---
title: 'Azure CLI-Script voorbeeld: ophalen van de hostnaam, poorten en sleutels voor Azure Cache voor Redis | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: ophalen van de hostnaam, poorten en sleutels voor een Azure-Cache voor Redis-exemplaar'
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: ff410db561879089c4c1f20acb7cb349f0484fda
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233826"
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
| [AZ redis list-keys](https://docs.microsoft.com/cli/azure/redis) | Toegangssleutel ophalen voor een Azure-Cache voor Redis-exemplaar. |


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende Azure-Cache voor Redis CLI-voorbeeldscripts vindt u de [Azure Cache voor Redis-documentatie](../cli-samples.md).
