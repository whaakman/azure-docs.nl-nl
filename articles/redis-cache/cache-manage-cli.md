---
title: Azure Redis-Cache met behulp van Azure classic CLI beheren | Microsoft Docs
description: Informatie over het installeren van de klassieke Azure-CLI op elk platform, het gebruik van deze verbinding maken met uw Azure-account en het maken en een Redis-cache beheren vanuit de klassieke CLI.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: 0e8bbaad920f35028c51641779a3272f73f81f37
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978400"
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-classic-cli"></a>Over het maken en beheren van Azure Redis Cache met behulp van de klassieke Azure-CLI
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [CLI van Azure classic](cache-manage-cli.md)
>

De klassieke Azure-CLI is een uitstekende manier om uw Azure-infrastructuur beheren vanaf elk platform. Dit artikel ziet u hoe u uw Azure Redis-Cache-instanties met de klassieke Azure-CLI maken en beheren.

[!INCLUDE [outdated-cli-content](../../includes/contains-classic-cli-content.md)]
> [!NOTE]
> Zie voor de nieuwste Azure CLI-voorbeeldscripts [voorbeelden van CLI van Azure Redis-cache](cli-samples.md).

## <a name="prerequisites"></a>Vereisten
Als u wilt maken en beheren van Azure Redis Cache-exemplaren met behulp van Azure classic CLI, moet u de volgende stappen uitvoeren.

* U moet een Azure-account hebben. Als u niet hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) in een paar seconden.
* [De klassieke Azure-CLI installeren](../cli-install-nodejs.md).
* Verbinding maken met de Azure CLI-installatie met een persoonlijke Azure-account of met een werk of school van Azure-account op en meld u vanuit de klassieke CLI met de `azure login` opdracht.
* Ga voordat u een van de volgende opdrachten uitvoert, de klassieke CLI in Resource Manager-modus door het uitvoeren van de `azure config mode arm` opdracht. Zie voor meer informatie, [de klassieke Azure CLI gebruiken voor het beheren van Azure-resources en resourcegroepen](../xplat-cli-azure-resource-manager.md).

## <a name="redis-cache-properties"></a>Redis-Cache-eigenschappen
De volgende eigenschappen worden gebruikt bij het maken en bijwerken van Redis Cache-exemplaren.

| Eigenschap | Switch | Beschrijving |
| --- | --- | --- |
| naam |-n-,--naam |De naam van de Redis-Cache. |
| resourcegroep |-g-,--resourcegroep |De naam van de resourcegroep. |
| location |l-,--locatie |Locatie voor het maken van de cache. |
| grootte |-z, -grootte |Grootte van de Redis-Cache. Geldige waarden: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| SKU |-x-,--sku |Redis-SKU. Moet een van: [Basic, Standard, Premium] |
| EnableNonSslPort |-e-,--inschakelen-niet-ssl-poort |EnableNonSslPort-eigenschap van de Redis-Cache. Met deze markering toevoegen als u wilt de niet-SSL-poort voor uw cache inschakelen |
| Redis-configuratie |-c-,--redis-configuratie |Redis-configuratie. Voer een tekenreeks in JSON-indeling van de van configuratiesleutels en waarden die hier. Indeling: ' {' ":""," ":" '} ' |
| Redis-configuratie |-f-,--redis-configuratiebestand |Redis-configuratie. Geef het pad van een bestand met de van configuratiesleutels en waarden die hier. Indeling voor de bestandsvermelding: {"": "","": ""} |
| Aantal shards |-r-,--aantal shards |Het aantal Shards te maken op een Premium-Cluster-Cache met clustering. |
| Virtual Network |-v-,--virtueel netwerk |Hiermee geeft u de exacte ARM-resource-ID van het virtuele netwerk voor het implementeren van de redis-cache in bij het hosten van uw cache in een VNET. Voorbeeld van een indeling: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Sleuteltype |-t-,--key-type |Het type sleutel te vernieuwen. Geldige waarden: [primaire, secundaire] |
| StaticIP |-p-,--statische ip-< statische IP-> |Bij het hosten van uw cache in een VNET, geeft een uniek IP-adres in het subnet voor de cache. Als niet is opgegeven, wordt een gekozen voor u uit het subnet. |
| Subnet |t-,--subnet <subnet> |Bij het hosten van uw cache in een VNET, geeft de naam van het subnet waarin de cache implementeert. |
| VirtualNetwork |-v-,--virtueel netwerk < virtueel-netwerk > |Hiermee geeft u de exacte ARM-resource-ID van het virtuele netwerk voor het implementeren van de redis-cache in bij het hosten van uw cache in een VNET. Voorbeeld van een indeling: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonnement |-s-,--abonnement |De abonnements-id. |

## <a name="see-all-redis-cache-commands"></a>Zie alle Redis-Cache-opdrachten
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

## <a name="delete-an-existing-redis-cache"></a>Een bestaand Redis-Cache verwijderen
Als u wilt verwijderen van een Redis-Cache, gebruik de volgende opdracht:

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
Als u alle Redis-Caches binnen uw abonnement of resourcegroep, gebruik de volgende opdracht:

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

## <a name="show-properties-of-an-existing-redis-cache"></a>Eigenschappen van een bestaand Redis-Cache weergeven
Om weer te geven eigenschappen van een bestaand Redis-Cache, gebruikt u de volgende opdracht:

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

## <a name="change-settings-of-an-existing-redis-cache"></a>Instellingen van een bestaand Redis-Cache wijzigen
Om instellingen te wijzigen van een bestaand Redis-Cache, gebruik de volgende opdracht:

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

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>De verificatiesleutel vernieuwen voor een bestaand Redis-Cache
Als u wilt de verificatiesleutel vernieuwen voor een bestaand Redis-Cache, gebruik de volgende opdracht:

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

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Lijst met primaire en secundaire sleutels van een bestaand Redis-Cache
Gebruik de volgende opdracht aan lijst met primaire en secundaire sleutels van een bestaand Redis-Cache:

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
