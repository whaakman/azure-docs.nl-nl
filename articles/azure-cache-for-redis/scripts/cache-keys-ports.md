---
title: 'Azure CLI-Script voorbeeld: ophalen van de hostnaam, poorten en sleutels voor Azure Cache voor Redis | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: ophalen van de hostnaam, poorten en sleutels voor een Azure-Cache voor Redis-exemplaar'
services: azure-cache-for-redis
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
ms.openlocfilehash: f9a963ec81b78cfcc6ded7d8f35f4066f931e53e
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54847313"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>De hostnaam, poorten en sleutels voor Azure Cache voor Redis ophalen

In dit scenario leert u hoe u de hostnaam, poorten en sleutels die worden gebruikt om verbinding met een Azure-Cache voor Redis-exemplaar te halen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Uitleg van het script

Dit script worden de volgende opdrachten gebruikt om op te halen van de hostnaam, sleutels en poorten van een Azure-Cache voor Redis-exemplaar. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ redis show](https://docs.microsoft.com/cli/azure/redis) | Gegevens van een Azure-Cache voor Redis-exemplaar niet ophalen. |
| [AZ redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Toegangssleutel ophalen voor een Azure-Cache voor Redis-exemplaar. |


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende Azure-Cache voor Redis CLI-voorbeeldscripts vindt u de [Azure Cache voor Redis-documentatie](../cli-samples.md).
