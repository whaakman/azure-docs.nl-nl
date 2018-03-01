---
title: Azure CLI-voorbeeldscript - Linux-groep in Batch | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een Linux-groep maken en beheren in Batch
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: f0e6ca88217cf7acbfa2478bf3eba1f52d83001c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="cli-example-create-and-manage-a-linux-pool-in-azure-batch"></a>CLI-voorbeeld: een Linux-groep maken en beheren in Azure Batch

Dit script toont enkele van de opdrachten die in de Azure CLI beschikbaar zijn voor het maken en beheren van een groep Linux-rekenknooppunten in Azure Batch.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids versie Azure CLI 2.0.20 of later uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Linux Virtual Machine Pool")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Hiermee wordt de Batch-account gemaakt. |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Hiermee wordt authenticatie uitgevoerd met het opgegeven Batch-account voor verdere interactie met de CLI.  |
| [az batch pool node-agent-skus list](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az_batch_pool_node_agent_skus_list) | Hiermee maakt u een lijst met de beschikbare knooppuntagent-SKU's en Installatiekopiegegevens.  |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Hiermee wordt een groep rekenknooppunten gemaakt.  |
| [az batch pool resize](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_resize) | Hiermee wordt het aantal actieve VM's in de opgegeven groep aangepast.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Hiermee worden de eigenschappen van een groep opgehaald.  |
| [az batch node list](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_list) | Hiermee wordt een lijst gemaakt van alle rekenknooppunten in de opgegeven groep.  |
| [az batch node reboot](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_reboot) | Hiermee wordt het opgegeven rekenknooppunt opnieuw opgestart.  |
| [az batch node delete](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_delete) | Hiermee worden de vermelde knooppunten uit de opgegeven groep verwijderd.  |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Zie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview) voor meer informatie over de Azure CLI.
