Voor het maken van een verbinding met een exemplaar van Azure Redis Cache, hebben de cache-clients de hostnaam, poorten en sleutels van de cache nodig. Sommige clients gebruiken enigszins andere namen om naar deze items te verwijzen. U kunt deze informatie ophalen in Azure Portal of met behulp van opdrachtregelprogramma's zoals Azure CLI.

### <a name="retrieve-host-name-ports-and-access-keys-using-the-azure-portal"></a>Hostnaam, poorten en toegangssleutels ophalen via Azure Portal
Als u de hostnaam, poorten en toegangssleutels wilt ophalen via Azure Portal, [bladert](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) u naar de cache in [Azure Portal](https://portal.azure.com) en klikt u op **Toegangssleutels** en **Eigenschappen** in het **menu Resource**. 

![Instellingen van Redis-cache](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Hostnaam, poorten en toegangssleutels ophalen via Azure CLI
Als u de hostnaam en poorten met Azure CLI 2.0 wilt ophalen, kunt u [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) oproepen. Als u de sleutels wilt ophalen, kunt u [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) oproepen. Het volgende script roept deze twee opdrachten aan en echoot de hostnaam, poorten en sleutels naar de console.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Zie [Get the hostname, ports, and keys for Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md) (De hostnaam, poorten en sleutels voor Azure Redis Cache ophalen) voor meer informatie over dit script. Zie [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Azure CLI 2.0 installeren) en [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) (Aan de slag met Azure CLI 2.0) voor meer informatie over Azure CLI 2.0.
