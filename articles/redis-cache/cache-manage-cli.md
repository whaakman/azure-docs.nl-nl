---
title: Beheren van Azure Redis-Cache met behulp van Azure CLI | Microsoft Docs
description: Informatie over hoe Azure CLI installeren op elk platform, het gebruik van deze verbinding maken met uw Azure-account en het maken en beheren van een Redis-cache van de Azure CLI.
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: fdb0989af2215166b69f10474a0d22aab7b4d593
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Het maken en beheren van Azure Redis-Cache met behulp van de Azure-opdrachtregelinterface (Azure CLI)
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure-CLI](cache-manage-cli.md)
>
>

De Azure CLI is een uitstekende manier om uw Azure-infrastructuur beheren vanaf een willekeurig platform. In dit artikel leest u hoe maken en beheren van uw Azure Redis-Cache-exemplaren die gebruikmaken van de Azure CLI.

> [!NOTE]
> In dit artikel is van toepassing op een eerdere versie van Azure CLI. Zie voor de nieuwste Azure CLI 2.0-voorbeeldscripts [CLI van Azure Redis-cache-voorbeelden](cli-samples.md).
> 
> 

## <a name="prerequisites"></a>Vereisten
Als u wilt maken en beheren van Azure Redis-Cache-exemplaren die gebruikmaken van Azure CLI, moet u de volgende stappen uitvoeren.

* U moet een Azure-account hebben. Als u niet hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) in slechts enkele ogenblikken.
* [Installeer de Azure CLI](../cli-install-nodejs.md).
* Verbinding maken met de Azure CLI-installatie met een persoonlijke Azure-account of met een werk of school Azure-account en zich aanmelden vanaf de Azure CLI met behulp van de `azure login` opdracht. Als u wilt weten over de verschillen en ervoor kiest, Zie [verbinding maken met een Azure-abonnement met de Azure-opdrachtregelinterface (Azure CLI)](/cli/azure/authenticate-azure-cli).
* Voordat u een van de volgende opdrachten, schakelt u de Azure CLI in de modus Resource Manager door het uitvoeren van de `azure config mode arm` opdracht. Zie voor meer informatie [de Azure CLI gebruiken voor het beheren van Azure-resources en resourcegroepen](../xplat-cli-azure-resource-manager.md).

## <a name="redis-cache-properties"></a>Redis-Cache-eigenschappen
De volgende eigenschappen worden gebruikt bij het maken en bijwerken van Redis-Cache-exemplaren.

| Eigenschap | Switch | Beschrijving |
| --- | --- | --- |
| naam |-n,--naam |Naam van de Redis-Cache. |
| resourcegroep |-g,--resourcegroep |Naam van de resourcegroep. |
| location |-l,--locatie |Locatie voor het maken van de cache. |
| grootte |-z,--grootte |Grootte van de Redis-Cache. Geldige waarden: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| sku |-x, --sku |Redis SKU. Moet een van: [Basic, Standard, Premium] |
| EnableNonSslPort |-e,--enable-niet-ssl-poort |EnableNonSslPort-eigenschap van de Redis-Cache. Deze vlag toevoegen als u wilt de niet-SSL-poort voor uw cache inschakelen |
| Configuratie van redis |-c,--redis-configuratie |Redis-configuratie. Geef een string in JSON-indeling van de van configuratiesleutels en waarden die hier. Indeling: ' {' ': "","": ""} " |
| Configuratie van redis |-f,--redis-configuratiebestand |Redis-configuratie. Geef het pad van een bestand met configuratiesleutels en waarden die hier. Indeling voor de bestandsvermelding: {"": "","": ""} |
| Aantal shard |-r,--shard-telling |Het aantal Shards maken op een Cluster Premium Cache met clustering. |
| Virtueel netwerk |-v,--virtueel netwerk |Hiermee geeft u de exacte ARM resource-ID van het virtuele netwerk voor het implementeren van de redis-cache in bij het hosten van uw cache in een VNET. Voorbeeld van de indeling: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| sleuteltype |-t,--sleutel-type |Het type van de sleutel te vernieuwen. Geldige waarden: [primaire, secundaire] |
| StaticIP |-p,--statisch ip-< statisch ip-> |Bij het hosten van uw cache in een VNET, geeft een uniek IP-adres in het subnet voor de cache. Als niet wordt opgegeven, wordt een gekozen voor u van het subnet. |
| Subnet |t,--subnet<subnet> |Bij het hosten van uw cache in een VNET, geeft de naam van het subnet waarin de cache te implementeren. |
| VirtualNetwork |-v,--virtueel netwerk < virtuele-netwerk > |Hiermee geeft u de exacte ARM resource-ID van het virtuele netwerk voor het implementeren van de redis-cache in bij het hosten van uw cache in een VNET. Voorbeeld van de indeling: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonnement |-s,--abonnement |De abonnements-id. |

## <a name="see-all-redis-cache-commands"></a>Zie alle opdrachten voor Redis-Cache
Als alle opdrachten voor Redis-Cache en de bijbehorende parameters wilt weergeven, gebruikt de `azure rediscache -h` opdracht.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Een Redis-cache maken
Gebruik de volgende opdracht voor het maken van een Redis-Cache:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Voor meer informatie over deze opdracht uitvoert de `azure rediscache create -h` opdracht.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Verwijderen van een bestaande Redis-Cache
Gebruik de volgende opdracht voor het verwijderen van een Redis-Cache:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Voor meer informatie over deze opdracht uitvoert de `azure rediscache delete -h` opdracht.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Lijst van alle Redis-Caches binnen uw abonnement of resourcegroep
Als alle Redis-Caches binnen uw abonnement of resourcegroep wilt weergeven, gebruikt u de volgende opdracht:

    azure rediscache list [options]

Voor meer informatie over deze opdracht uitvoert de `azure rediscache list -h` opdracht.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Eigenschappen van een bestaande Redis-Cache weergeven
Als u wilt de eigenschappen van een bestaande Redis-Cache weergeven, moet u de volgende opdracht gebruiken:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Voor meer informatie over deze opdracht uitvoert de `azure rediscache show -h` opdracht.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a>Instellingen van een bestaande Redis-Cache wijzigen
Om instellingen te wijzigen van een bestaande Redis-Cache, gebruik de volgende opdracht:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Voor meer informatie over deze opdracht uitvoert de `azure rediscache set -h` opdracht.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Vernieuwen van de verificatiesleutel voor een bestaande Redis-Cache
Als u wilt vernieuwen van de verificatiesleutel voor een bestaande Redis-Cache, gebruik de volgende opdracht:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Geef `Primary` of `Secondary` voor `key-type`.

Voor meer informatie over deze opdracht uitvoert de `azure rediscache renew-key -h` opdracht.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Lijst met primaire en secundaire sleutels van een bestaande Redis-Cache
Gebruik de volgende opdracht aan lijst primaire en secundaire sleutels van een bestaande Redis-Cache:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Voor meer informatie over deze opdracht uitvoert de `azure rediscache list-keys -h` opdracht.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
