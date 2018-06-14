---
title: Azure CLI-voorbeeldscript - Get de hostnaam, poorten en sleutels voor Azure Redis-Cache | Microsoft Docs
description: Azure CLI-voorbeeldscript - Get de hostnaam, poorten en sleutels voor een Azure Redis-Cache-exemplaar
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 9a2a6a7ed4bb56f93c965a5bfd24b21368117c7b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846448"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Ophalen van de hostnaam, poorten en sleutels voor Azure Redis-Cache

In dit scenario u informatie over het ophalen van de hostnaam, poorten en sleutels die verbinding maken met een Azure Redis-Cache-exemplaar worden gebruikt.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten voor het ophalen van de hostnaam, sleutels en poorten van een Azure Redis-Cache-exemplaar. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ redis weergeven](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Ophalen van gegevens van een Azure Redis-Cache-exemplaar. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Sneltoetsen voor een Azure Redis-Cache-exemplaar worden opgehaald. |


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende voorbeelden van Azure Redis-Cache CLI-script kunnen worden gevonden in de [documentatie van Azure Redis-Cache](../cli-samples.md).