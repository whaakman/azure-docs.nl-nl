---
title: Meer replica's voor Azure Database voor PostgreSQL vanuit de Azure CLI beheren
description: Meer informatie over het beheren van Azure Database for PostgreSQL lezen replica's van de Azure CLI.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: 21408f87c4446ebad4092cb982179c7d78ea9e32
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58847761"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Maken en beheren van meer replica's van de Azure CLI

In dit artikel leert u hoe u maken en beheren van lezen-replica's in Azure Database for PostgreSQL via Azure CLI. Zie voor meer informatie over meer replica's, de [overzicht](concepts-read-replicas.md).

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

De `az mysql server replica create` opdracht moet de volgende parameters:

| Instelling | Voorbeeldwaarde | Description  |
| --- | --- | --- |
| resource-group | myResourceGroup |  De resourcegroep waar de replica-server wordt gemaakt.  |
| naam | mydemoserver-replica | De naam van de nieuwe replicaserver die is gemaakt. |
| source-server | mydemoserver | De naam of resource-ID van de bestaande hoofd-server om te repliceren van. |

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Als u dit nog niet hebt ingesteld de `azure.replication_support` parameter **REPLICA** op een algemeen gebruik of geoptimaliseerd voor geheugen-master server en de server opnieuw opgestart, wordt er een fout. Deze twee stappen voltooien voordat u een replica maakt.

Een replica wordt gemaakt met behulp van de configuratie van de dezelfde server als het model. Nadat een replica is gemaakt, verschillende instellingen van de hoofdserver onafhankelijk van elkaar kunnen worden gewijzigd: compute genereren, vCores, opslag en back-up maken van bewaarperiode. De prijscategorie kan ook afzonderlijk worden gewijzigd met uitzondering van of naar de Basic-laag.

> [!IMPORTANT]
> Werk de configuratie van de replica op gelijke of grotere waarden voordat een hoofd-server-configuratie is bijgewerkt met nieuwe waarden. Deze actie zorgt ervoor dat de replica kunt houden met eventuele wijzigingen in het model.

## <a name="list-replicas"></a>Lijst met replica 's
U kunt de lijst van replica's van een hoofd-server weergeven.

```azurecli-interactive
az postgres server replica stop --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Replicatie naar een replica-server stoppen
U kunt replicatie tussen een hoofd-server en een lezen replica stoppen.

Na het stoppen van replicatie naar een hoofd-server en een replica lezen, kunnen deze kan niet ongedaan worden gemaakt. De replica voor lezen wordt een zelfstandige server die ondersteuning biedt voor zowel lees- en schrijfbewerkingen. De zelfstandige server kan niet opnieuw worden gemaakt in een replica.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Een server of replica verwijderen
Als u wilt een server of replica verwijdert, moet u de dezelfde opdracht over het verwijderen van een zelfstandige Azure Database for PostgreSQL-server gebruiken. 

Wanneer u een hoofdserver verwijdert, wordt replicatie naar alle lezen-replica's gestopt. De lezen-replica's worden zelfstandige servers die bieden nu ondersteuning voor zowel lees- en schrijfbewerkingen.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [lezen-replica's in Azure Database for PostgreSQL](concepts-read-replicas.md).