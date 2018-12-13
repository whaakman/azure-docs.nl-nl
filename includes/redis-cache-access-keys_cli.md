---
title: bestand opnemen
description: bestand opnemen
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 03abefb50ddafca599edc289123c5fb3090971a2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091877"
---
### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Hostnaam, poorten en toegangssleutels ophalen via Azure CLI

Om op te halen van de hostnaam en poorten met de Azure CLI kunt u bellen [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show), en op te halen van de sleutels die u kunt aanroepen [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys). Het volgende script roept deze twee opdrachten aan en echoot de hostnaam, poorten en sleutels naar de console.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Cache for Redis instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Cache for Redis instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Zie voor meer informatie over dit script [ophalen van de hostnaam, poorten en sleutels voor Azure Cache voor Redis](../articles/azure-cache-for-redis/scripts/cache-keys-ports.md). Zie voor meer informatie over Azure-CLI [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) en [aan de slag met Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
