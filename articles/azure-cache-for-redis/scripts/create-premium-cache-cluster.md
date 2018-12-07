---
title: Azure CLI-voorbeeldscript - het maken van een Premium Azure Cache voor Redis met clustering | Microsoft Docs
description: Azure CLI-voorbeeldscript - het maken van een Premium-laag van Azure Cache voor Redis met clustering
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 727af1b64f43189d3cfcee39d10bfb6920323ffb
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53019470"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Maken van een Premium Azure Cache voor Redis met clustering

In dit scenario leert u hoe u een Premium-laag van 6 GB Azure-Cache maken voor Redis Clustering is ingeschakeld en twee shards.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Dit script worden de volgende opdrachten gebruikt om een resourcegroep te maken en een Premium-laag Azure Redis Cache met clustering inschakelen. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ redis maken](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Azure Cache voor Redis-exemplaar maken. |


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende Azure-Cache voor Redis CLI-voorbeeldscripts vindt u de [Azure Cache voor Redis-documentatie](../cli-samples.md).