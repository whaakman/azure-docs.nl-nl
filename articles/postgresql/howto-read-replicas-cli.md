---
title: Meer replica's voor Azure Database voor PostgreSQL - één Server van de Azure CLI beheren
description: Informatie over het beheren van lezen-replica's in Azure Database voor PostgreSQL - één Server van de Azure CLI.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/28/2019
ms.openlocfilehash: 9a6a1a744a8441d2f082d4d14a3aba8aa1cfc09e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306023"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Maken en beheren van meer replica's van de Azure CLI

In dit artikel leert u hoe u maken en beheren van lezen-replica's in Azure Database for PostgreSQL via Azure CLI. Zie voor meer informatie over meer replica's, de [overzicht](concepts-read-replicas.md).

> [!IMPORTANT]
> U kunt een lezen replica maken in dezelfde regio als de hoofd-server of in een andere Azure-regio van uw keuze. Regio-overschrijdende replicatie is momenteel in openbare preview.

## <a name="prerequisites"></a>Vereisten
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-up-azure-cli.md) moet de hoofd-server.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer de opdracht `az --version` uit om de geïnstalleerde versie te zien. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 


## <a name="prepare-the-master-server"></a>De hoofd-server voorbereiden
Deze stappen moeten worden gebruikt om voor te bereiden van een hoofd-server in de lagen algemeen gebruik en geoptimaliseerd voor geheugen.

De `azure.replication_support` parameter moet worden ingesteld op **REPLICA** op de hoofd-server. Als deze parameter static wordt gewijzigd, is een server opnieuw opstarten vereist voor de wijziging door te voeren.

1. Stel `azure.replication_support` naar REPLICA.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Start opnieuw op om toe te passen van de wijziging naar de server.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

## <a name="create-a-read-replica"></a>Maken van een replica lezen

De [az postgres server-replica maken](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) opdracht moet de volgende parameters:

| Instelling | Voorbeeldwaarde | Description  |
| --- | --- | --- |
| resource-group | myResourceGroup |  De resourcegroep waar de replica-server wordt gemaakt.  |
| name | mydemoserver-replica | De naam van de nieuwe replicaserver die is gemaakt. |
| source-server | mydemoserver | De naam of resource-ID van de bestaande hoofd-server om te repliceren van. |

De replica is gemaakt in het onderstaande voorbeeld CLI in dezelfde regio als het model.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Maken van een kruis regio replica lezen, gebruikt u de `--location` parameter. De volgende CLI-voorbeeld maakt de replica in VS-West.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location westus
```

Als u dit nog niet hebt ingesteld de `azure.replication_support` parameter **REPLICA** op een algemeen gebruik of geoptimaliseerd voor geheugen-master server en de server opnieuw opgestart, wordt er een fout. Deze twee stappen voltooien voordat u een replica maakt.

Een replica wordt gemaakt met behulp van de configuratie van de dezelfde server als het model. Nadat een replica is gemaakt, verschillende instellingen van de hoofdserver onafhankelijk van elkaar kunnen worden gewijzigd: compute genereren, vCores, opslag en back-up maken van bewaarperiode. De prijscategorie kan ook afzonderlijk worden gewijzigd met uitzondering van of naar de Basic-laag.

> [!IMPORTANT]
> Werk de configuratie van de replica op gelijke of grotere waarden voordat een hoofd-server-configuratie is bijgewerkt met nieuwe waarden. Deze actie zorgt ervoor dat de replica kunt houden met eventuele wijzigingen in het model.

## <a name="list-replicas"></a>Lijst met replica 's
U kunt de lijst van replica's van een hoofdserver weergeven met behulp van [lijst az postgres server-replica](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) opdracht.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Replicatie naar een replica-server stoppen
U kunt replicatie tussen een hoofd-server en een lezen replica stoppen met [az postgres server replica stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) opdracht.

Na het stoppen van replicatie naar een hoofd-server en een replica lezen, kunnen deze kan niet ongedaan worden gemaakt. De replica voor lezen wordt een zelfstandige server die ondersteuning biedt voor zowel lees- en schrijfbewerkingen. De zelfstandige server kan niet opnieuw worden gemaakt in een replica.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Een server of replica verwijderen
Als u wilt verwijderen van een model of de replica-server, gebruikt u de [az postgres server delete](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) opdracht.

Wanneer u een hoofdserver verwijdert, wordt replicatie naar alle lezen-replica's gestopt. De lezen-replica's worden zelfstandige servers die bieden nu ondersteuning voor zowel lees- en schrijfbewerkingen.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [lezen-replica's in Azure Database for PostgreSQL](concepts-read-replicas.md).