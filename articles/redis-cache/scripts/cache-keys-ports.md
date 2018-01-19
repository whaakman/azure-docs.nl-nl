---
title: Azure CLI-voorbeeldscript - Get de hostnaam, poorten en sleutels voor Azure Redis-Cache | Microsoft Docs
description: Azure CLI-voorbeeldscript - Get de hostnaam, poorten en sleutels voor een Azure Redis-Cache-exemplaar
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 6cfff74ed037835abb6d3e3975fe33d71b9730a3
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Ophalen van de hostnaam, poorten en sleutels voor Azure Redis-Cache

In dit scenario u informatie over het ophalen van de hostnaam, poorten en sleutels die verbinding maken met een Azure Redis-Cache-exemplaar worden gebruikt.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het ophalen van de hostnaam, sleutels en poorten van een Azure Redis-Cache-exemplaar. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ redis weergeven](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Ophalen van gegevens van een Azure Redis-Cache-exemplaar. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Sneltoetsen voor een Azure Redis-Cache-exemplaar worden opgehaald. |


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Redis-Cache CLI-script kunnen worden gevonden in de [documentatie van Azure Redis-Cache](../cli-samples.md).