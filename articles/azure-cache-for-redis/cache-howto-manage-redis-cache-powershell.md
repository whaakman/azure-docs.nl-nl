---
title: Azure-Cache beheren voor Redis met Azure PowerShell | Microsoft Docs
description: Informatie over het uitvoeren van beheertaken voor Azure Cache voor Redis met behulp van Azure PowerShell.
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: ffbd785126bbc204191554e5d62d642a582a3c8d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822558"
---
# <a name="manage-azure-cache-for-redis-with-azure-powershell"></a>Azure Cache voor Redis met Azure PowerShell beheren
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure-CLI](cache-manage-cli.md)
> 
> 

Dit onderwerp leest u hoe algemene taken is om uit te voeren zoals maken, bijwerken en schalen van uw Azure-Cache voor Redis-exemplaren, hoe u toegang tot sleutels opnieuw genereren en hoe u om informatie over uw caches weer te geven. Zie voor een volledige lijst van Azure Cache voor Redis-PowerShell-cmdlets, [Azure Cache voor Redis-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.rediscache/?view=azurermps-6.6.0).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Zie voor meer informatie over het klassieke implementatiemodel, [Azure Resource Manager en klassieke implementatie: Implementatiemodellen en de status van uw resources begrijpen](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="prerequisites"></a>Vereisten
Als u Azure PowerShell al hebt geïnstalleerd, hebt u Azure PowerShell versie 1.0.0 of hoger. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met de volgende opdracht bij de Azure PowerShell-opdrachtprompt.

    Get-Module azure | format-table version


Eerst moet u zich aanmelden Azure met deze opdracht.

    Connect-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het bijbehorende wachtwoord in het dialoogvenster van de aanmelding bij Microsoft Azure.

Als u meerdere Azure-abonnementen hebt, moet u vervolgens uw Azure-abonnement instellen. Een lijst van uw huidige abonnementen wilt bekijken, moet u deze opdracht uitvoeren.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Als u het abonnement, moet u de volgende opdracht uitvoeren. In het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Voordat u Windows PowerShell met Azure Resource Manager gebruiken kunt, moet u het volgende:

* Windows PowerShell versie 3.0 of 4.0. Als u wilt zien welke versie van Windows PowerShell, typ:`$PSVersionTable` en controleer of de waarde van `PSVersion` 3.0 of 4.0 is. Zie voor het installeren van een compatibele versie [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).

Als u gedetailleerde Help-informatie voor elke cmdlet die u in deze zelfstudie ziet, gebruikt u de cmdlet Get-Help.

    Get-Help <cmdlet-name> -Detailed

Als u bijvoorbeeld ondersteuning voor de `New-AzureRmRedisCache` cmdlet, type:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Verbinding maken met andere clouds
Standaard de Azure-omgeving is `AzureCloud`, die staat voor de globale Azure-cloud-exemplaar. Gebruiken voor verbinding met een ander exemplaar, de `Connect-AzureRmAccount` opdracht met de `-Environment` of -`EnvironmentName` opdrachtregelparameter met de gewenste omgeving of de naam van de omgeving.

Als u wilt zien van de lijst met beschikbare omgevingen, de `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbinding maken met de Azure Government-Cloud
Voor verbinding met de Azure Government-Cloud, gebruikt u een van de volgende opdrachten.

    Connect-AzureRmAccount -EnvironmentName AzureUSGovernment

of

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Voor het maken van een cache in de Azure Government-Cloud, gebruikt u een van de volgende locaties.

* USGov Virginia
* USGov Iowa

Zie voor meer informatie over de Azure Government-Cloud, [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) en [Ontwikkelaarshandleiding voor Microsoft Azure Government](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Verbinding maken met de Azure China-Cloud
Voor verbinding met de Azure China-Cloud, gebruikt u een van de volgende opdrachten.

    Connect-AzureRmAccount -EnvironmentName AzureChinaCloud

of

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Voor het maken van een cache in de Azure China-Cloud, gebruikt u een van de volgende locaties.

* China East
* China - noord

Zie voor meer informatie over de Azure China-Cloud, [AzureChinaCloud voor Azure uitgevoerd door 21Vianet in China](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>Verbinding maken met Microsoft Azure Duitsland
Voor verbinding met Microsoft Azure Duitsland, gebruikt u een van de volgende opdrachten.

    Connect-AzureRmAccount -EnvironmentName AzureGermanCloud


of

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

Voor het maken van een cache in Microsoft Azure Duitsland, gebruikt u een van de volgende locaties.

* Duitsland - centraal
* Duitsland - noordoost

Zie voor meer informatie over Microsoft Azure Duitsland, [Microsoft Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-cache-for-redis-powershell"></a>Eigenschappen die worden gebruikt voor Azure Cache voor Redis-PowerShell
De volgende tabel bevat de eigenschappen en beschrijvingen voor de meest gebruikte parameters bij het maken en beheren van uw Azure-Cache voor instanties van Redis met behulp van Azure PowerShell.

| Parameter | Description | Standaard |
| --- | --- | --- |
| Name |Naam van de cache | |
| Locatie |Locatie van de cache | |
| ResourceGroupName |Naam van resourcegroep waarin u kunt de cache maken | |
| Grootte |De grootte van de cache. Geldige waarden zijn: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB |1 GB |
| ShardCount |Het aantal shards te maken bij het maken van een premium-cache met clustering is ingeschakeld. Geldige waarden zijn: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Hiermee geeft u de SKU van de cache. Geldige waarden zijn: Basic, Standard en Premium |Standard |
| RedisConfiguration |Hiermee geeft u een Redis-configuratie-instellingen. Zie voor meer informatie over elke instelling de volgende [RedisConfiguration eigenschappen](#redisconfiguration-properties) tabel. | |
| EnableNonSslPort |Geeft aan of de poort zonder SSL is ingeschakeld. |False |
| MaxMemoryPolicy |Deze parameter is afgeschaft - RedisConfiguration in plaats daarvan gebruik. | |
| StaticIP |Bij het hosten van uw cache in een VNET, geeft een uniek IP-adres in het subnet voor de cache. Als niet is opgegeven, wordt een gekozen voor u uit het subnet. | |
| Subnet |Bij het hosten van uw cache in een VNET, geeft de naam van het subnet waarin de cache implementeert. | |
| VirtualNetwork |Bij het hosten van uw cache in een VNET, geeft u de resource-ID van het VNET waarin u kunt de cache te implementeren. | |
| KeyType |Hiermee geeft u op welke toegangssleutel opnieuw genereren tijdens het vernieuwen van toegangssleutels. Geldige waarden zijn: Primary, Secondary | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration eigenschappen
| Eigenschap | Description | Prijscategorieën |
| --- | --- | --- |
| de RDB-back-up-ingeschakeld |Of [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) is ingeschakeld |Alleen Premium |
| de RDB-opslag-connection-string |De verbindingsreeks naar het opslagaccount voor [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) |Alleen Premium |
| de RDB-back-up-frequentie |De back-upfrequentie voor [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) |Alleen Premium |
| maxmemory-reserved |Hiermee configureert u de [geheugen gereserveerd](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor niet-cache-processen |Standard en Premium |
| maxmemory-policy |Hiermee configureert u de [verwijderingsbeleid](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor de cache |Alle Prijscategorieën |
| op de hoogte stellen-keyspace-gebeurtenissen |Hiermee configureert u [keyspace-meldingen](cache-configure.md#keyspace-notifications-advanced-settings) |Standard en Premium |
| hash-max-ziplist-entries |Hiermee configureert u [geheugenoptimalisatie](https://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| hash-max-ziplist-value |Hiermee configureert u [geheugenoptimalisatie](https://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| set-max-intset-entries |Hiermee configureert u [geheugenoptimalisatie](https://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| zset-max-ziplist-entries |Hiermee configureert u [geheugenoptimalisatie](https://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| zset-max-ziplist-value |Hiermee configureert u [geheugenoptimalisatie](https://redis.io/topics/memory-optimization) voor kleine cumulatieve gegevenstypen |Standard en Premium |
| databases |Hiermee configureert u het aantal databases. Deze eigenschap kan alleen tijdens het maken van cache worden geconfigureerd. |Standard en Premium |

## <a name="to-create-an-azure-cache-for-redis"></a>Een Azure-Cache maken voor Redis
Nieuwe Azure-Cache voor Redis-exemplaren worden gemaakt met behulp van de [New-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/new-azurermrediscache?view=azurermps-6.6.0) cmdlet.

> [!IMPORTANT]
> De eerste keer dat u een Azure-Cache maken voor Redis in een abonnement met behulp van de Azure portal, de portal registreert de `Microsoft.Cache` naamruimte voor dat abonnement. Als u probeert te maken van de eerste Azure-Cache voor Redis in een abonnement met behulp van PowerShell, moet u eerst registreren die naamruimte met de volgende opdracht. anders cmdlets zoals `New-AzureRmRedisCache` en `Get-AzureRmRedisCache` mislukken.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Voor een overzicht van de parameters en hun beschrijvingen voor `New-AzureRmRedisCache`, voer de volgende opdracht uit.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new Azure Cache for Redis.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new Azure Cache for Redis.


    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the Azure Cache for Redis.

        -Location <String>
            Location in which to create the Azure Cache for Redis.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the Azure Cache for Redis. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of Azure Cache for Redis. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Cache for Redis. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the Azure Cache for Redis in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Voor het maken van een cache met standaard-parameters, moet u de volgende opdracht uitvoeren.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, en `Location` vereiste parameters zijn, maar de rest zijn optioneel en standaardwaarden. Met de vorige opdracht maakt u een Standard-SKU Azure Cache voor Redis-exemplaar met de opgegeven naam, locatie en resourcegroep-groep die is 1 GB groot zijn met de poort zonder SSL is uitgeschakeld.

Maken van een premium-cache, een grootte van P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), geef P3 (26 GB - 260 GB), of P4 (53 GB - 530 GB). Om in te schakelen clustering, geeft u een shard aantal met de `ShardCount` parameter. Het volgende voorbeeld wordt een P1 premium-cache met 3 shards. Een cache van de premium P1 6 GB groot is en omdat we drie shards opgegeven de totale grootte is 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Waarden opgeven voor de `RedisConfiguration` parameter, plaatst u de waarden in `{}` als sleutel/waarde-paren, zoals `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Het volgende voorbeeld wordt een standaard 1 GB cache met `allkeys-random` maxmemory-beleid en de keyspace meldingen geconfigureerd met `KEA`. Zie voor meer informatie, [Keyspace-meldingen (geavanceerde instellingen)](cache-configure.md#keyspace-notifications-advanced-settings) en [geheugen beleid](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Het configureren van de databases instellen tijdens het maken van cache
De `databases` instelling alleen tijdens het maken van de cache kan worden geconfigureerd. Het volgende voorbeeld wordt een premium P3 (26 GB)-cache met 48 databases met behulp van de [New-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/New-AzureRmRedisCache?view=azurermps-6.6.0) cmdlet.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Voor meer informatie over de `databases` eigenschap, Zie [standaard Azure Cache voor configuratie van Redis-server](cache-configure.md#default-redis-server-configuration). Voor meer informatie over het maken van een cache met de [New-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/new-azurermrediscache?view=azurermps-6.6.0) cmdlet, Zie de vorige te maken van een Azure-Cache voor Redis-sectie.

## <a name="to-update-an-azure-cache-for-redis"></a>Bijwerken van een Azure-Cache voor Redis
Azure voor instanties van Redis-Cache zijn bijgewerkt met de [Set-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Set-AzureRmRedisCache?view=azurermps-6.6.0) cmdlet.

Voor een overzicht van de parameters en hun beschrijvingen voor `Set-AzureRmRedisCache`, voer de volgende opdracht uit.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set Azure Cache for Redis updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets Azure Cache for Redis parameters.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the Azure Cache for Redis. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of Azure Cache for Redis. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Cache for Redis. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

De `Set-AzureRmRedisCache` cmdlet kan worden gebruikt voor het bijwerken van eigenschappen zoals `Size`, `Sku`, `EnableNonSslPort`, en de `RedisConfiguration` waarden. 

De volgende opdracht werkt de maxmemory-beleid voor de Azure-Cache voor Redis met de naam myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-an-azure-cache-for-redis"></a>Voor het schalen van een Azure-Cache voor Redis
`Set-AzureRmRedisCache` kan worden gebruikt om de schaal van een Azure-Cache voor Redis-exemplaar als de `Size`, `Sku`, of `ShardCount` eigenschappen worden gewijzigd. 

> [!NOTE]
> Schalen van een cache met behulp van PowerShell is onderhevig aan de dezelfde beperkingen en richtlijnen als het schalen van een cache van de Azure-portal. U kunt schalen naar een andere prijscategorie met de volgende beperkingen.
> 
> * U kan niet uit een hogere prijscategorie schalen in een lagere prijscategorie.
> * Kan niet worden geschaald van een **Premium** omlaag naar de cache een **Standard** of een **Basic** cache.
> * Kan niet worden geschaald van een **Standard** omlaag naar de cache een **Basic** cache.
> * Kunt u de schaal van een **Basic** in de cache op een **Standard** cache, maar u de grootte op hetzelfde moment niet wijzigen. Als u een andere grootte nodig hebt, kunt u de volgende vergroten/verkleinen bewerking naar de gewenste grootte kunt doen.
> * Kan niet worden geschaald van een **Basic** rechtstreeks naar de cache een **Premium** cache. U moet schalen van **Basic** te **Standard** in één bewerking voor vergroten/verkleinen en klik in **Standard** te **Premium** in een latere schalen de bewerking.
> * U kan niet schalen van een groter formaat omlaag naar de **C0 (250 MB)** grootte.
> 
> Zie voor meer informatie, [hoe Scale Azure Cache voor Redis](cache-how-to-scale.md).
> 
> 

Het volgende voorbeeld laat zien hoe de schaal van een cache met de naam `myCache` tot een 2,5 GB-cache. Houd er rekening mee dat deze opdracht voor zowel een Basislidmaatschap of een standaard-cache werkt.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nadat u deze opdracht is uitgevoerd, de status van de cache wordt geretourneerd (vergelijkbaar met aanroepen `Get-AzureRmRedisCache`). Houd er rekening mee dat de `ProvisioningState` is `Scaling`.

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

Wanneer de vergroten/verkleinen bewerking voltooid is, de `ProvisioningState` wordt gewijzigd in `Succeeded`. Als u wilt maken van de volgende vergroten/verkleinen bewerking, zoals het wijzigen van Basic naar Standard en vervolgens wijzigen van de grootte, moet u wachten totdat de vorige bewerking voltooid is of u een vergelijkbaar met de volgende fout ontvangt.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-an-azure-cache-for-redis"></a>Voor informatie over een Azure-Cache voor Redis
U kunt informatie over het gebruik van een cache ophalen de [Get-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/get-azurermrediscache?view=azurermps-6.6.0) cmdlet.

Voor een overzicht van de parameters en hun beschrijvingen voor `Get-AzureRmRedisCache`, voer de volgende opdracht uit.

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
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Uitvoeren als u wilt retourneren informatie over alle caches in het huidige abonnement, `Get-AzureRmRedisCache` zonder parameters.

    Get-AzureRmRedisCache

Uitvoeren als u wilt retourneren informatie over alle caches in een specifieke resourcegroep, `Get-AzureRmRedisCache` met de `ResourceGroupName` parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Uitvoeren om informatie te krijgen over een specifieke cache, `Get-AzureRmRedisCache` met de `Name` parameter met de naam van de cache en de `ResourceGroupName` parameter met de resourcegroep met deze cache.

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

## <a name="to-retrieve-the-access-keys-for-an-azure-cache-for-redis"></a>De toegangssleutel voor een Azure-Cache voor Redis ophalen
Als u wilt de toegangssleutels voor uw cache ophalen, kunt u de [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-6.6.0) cmdlet.

Voor een overzicht van de parameters en hun beschrijvingen voor `Get-AzureRmRedisCacheKey`, voer de volgende opdracht uit.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified Azure Cache for Redis.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Als wilt ophalen van de sleutels voor uw cache, roept de `Get-AzureRmRedisCacheKey` cmdlet en geef de naam van de cache met de naam van de resourcegroep met de cache.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-azure-cache-for-redis"></a>Opnieuw genereren van toegangssleutels voor uw Azure-Cache voor Redis
Als u wilt genereren van de toegangssleutels voor uw cache, kunt u de [New-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/New-AzureRmRedisCacheKey?view=azurermps-6.6.0) cmdlet.

Voor een overzicht van de parameters en hun beschrijvingen voor `New-AzureRmRedisCacheKey`, voer de volgende opdracht uit.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of an Azure Cache for Redis.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of an Azure Cache for Redis.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis.

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
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Als u wilt genereren van de primaire of secundaire sleutel voor uw cache, Roep de `New-AzureRmRedisCacheKey` cmdlet en geef de naam, resourcegroep, en opgeven of `Primary` of `Secondary` voor de `KeyType` parameter. In het volgende voorbeeld wordt de secundaire toegangssleutel voor een cache opnieuw gegenereerd.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for Azure Cache for Redis 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-an-azure-cache-for-redis"></a>Verwijderen van een Azure-Cache voor Redis
Als u wilt verwijderen van een Azure-Cache voor Redis, gebruikt u de [Remove-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/remove-azurermrediscache?view=azurermps-6.6.0) cmdlet.

Voor een overzicht van de parameters en hun beschrijvingen voor `Remove-AzureRmRedisCache`, voer de volgende opdracht uit.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove Azure Cache for Redis if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes an Azure Cache for Redis if it exists.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to remove.

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
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

In het volgende voorbeeld wordt de cache met de naam `myCache` wordt verwijderd.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove Azure Cache for Redis 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-an-azure-cache-for-redis"></a>Een Azure-Cache voor Redis importeren
U kunt gegevens importeren in een Azure-Cache voor het gebruik van Redis-exemplaar de `Import-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat. Zie voor meer informatie over Import/Export [importeren en exporteren van gegevens in Azure-Cache voor Redis](cache-how-to-import-export-data.md).
> 
> 

Voor een overzicht van de parameters en hun beschrijvingen voor `Import-AzureRmRedisCache`, voer de volgende opdracht uit.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Cache for Redis.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Cache for Redis.


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
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht importeert gegevens uit de blob die is opgegeven door de SAS-uri in de Azure-Cache voor Redis.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-an-azure-cache-for-redis"></a>Een Azure-Cache voor Redis exporteren
U kunt gegevens exporteren uit een Azure-Cache voor het gebruik van Redis-exemplaar de `Export-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat. Zie voor meer informatie over Import/Export [importeren en exporteren van gegevens in Azure-Cache voor Redis](cache-how-to-import-export-data.md).
> 
> 

Voor een overzicht van de parameters en hun beschrijvingen voor `Export-AzureRmRedisCache`, voer de volgende opdracht uit.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Cache for Redis to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Cache for Redis to a specified container.


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
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht exporteert gegevens in een Azure-Cache voor Redis-exemplaar naar de container die is opgegeven door de SAS-uri.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-an-azure-cache-for-redis"></a>Opnieuw opstarten van een Azure-Cache voor Redis
U kunt uw Azure-Cache voor het gebruik van Redis-exemplaar opnieuw de `Reset-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Opnieuw opstarten is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat. Zie voor meer informatie over uw cache opnieuw opstarten, [beheer in de Cache - opnieuw opstarten](cache-administration.md#reboot).
> 
> 

Voor een overzicht van de parameters en hun beschrijvingen voor `Reset-AzureRmRedisCache`, voer de volgende opdracht uit.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Cache for Redis instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Cache for Redis instance.


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
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht uit beide knooppunten van de opgegeven cache opnieuw is opgestart.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over het gebruik van Windows PowerShell met Azure:

* [Azure Cache voor Redis-cmdlet-documentatie op MSDN](https://docs.microsoft.com/powershell/module/azurerm.rediscache/?view=azurermps-6.6.0)
* [Azure Resource Manager-Cmdlets](https://go.microsoft.com/fwlink/?LinkID=394765): Leer hoe u met de cmdlets in de Azure Resource Manager-module.
* [Uw Azure-resources beheren met behulp van resourcegroepen](../azure-resource-manager/resource-group-template-deploy-portal.md): Informatie over het maken en beheren van resourcegroepen in Azure portal.
* [Azure-blog](https://azure.microsoft.com/blog/): Meer informatie over nieuwe functies in Azure.
* [Windows PowerShell-blog](https://blogs.msdn.com/powershell): Meer informatie over nieuwe functies in Windows PowerShell.
* ["Hey, Scripting Guy!" Blog](https://blogs.technet.com/b/heyscriptingguy/): Ontvang echte tips en trucs van de Windows PowerShell-community.

