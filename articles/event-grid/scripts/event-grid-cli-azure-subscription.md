---
title: Voorbeeld van Azure CLI-script - abonneren op een Azure-abonnement | Microsoft Docs
description: 'Voorbeeld van Azure CLI-script - abonneren op een Azure-abonnement '
services: event-grid
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2018
ms.author: tomfitz
ms.openlocfilehash: ac02c5515f598daf4aa91879af78341969718163
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31425890"
---
# <a name="subscribe-to-events-for-an-azure-subscription-with-azure-cli"></a>Abonneren op gebeurtenissen voor een Azure-abonnement met Azure CLI

Met dit script maakt u een Event Grid-abonnement op de gebeurtenissen voor een Azure-abonnement.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-azure-subscription/subscribe-to-azure-subscription.sh "Subscribe to Azure subscription")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script wordt de volgende opdracht gebruikt om het abonnement op de gebeurtenis te maken. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Hiermee wordt een Event Grid-abonnement gemaakt. |


## <a name="next-steps"></a>Volgende stappen

* Zie [Query Event Grid subscriptions](../query-event-subscriptions.md) (Query's uitvoeren op Event Grid-abonnementen) voor informatie over het uitvoeren van query's op abonnementen.
* Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.
