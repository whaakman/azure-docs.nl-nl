---
title: Azure CLI-voorbeeldscript - Een Batch-job uitvoeren | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een job uitvoeren met Batch
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: d63854b213b33a3c26358df17761999f5270d7a2
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843609"
---
# <a name="cli-example-run-a-job-and-tasks-with-azure-batch"></a>CLI-voorbeeld: een job en taken uitvoeren met Azure Batch

Met dit script maakt u een Batch-job en voegt u een reeks taken toe aan de job. U ziet ook hoe u een job en de taken daarvan kunt bewaken. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0.20 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

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
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Hiermee wordt een groep rekenknooppunten gemaakt.  |
| [az batch job create](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_create) | Hiermee wordt een Batch-taak gemaakt.  |
| [az batch task create](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_create) | Hiermee voegt u een taak toe aan de opgegeven Batch-taak.  |
| [az batch job set](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_set) | Hiermee worden eigenschappen van een Batch-taak bijgewerkt.  |
| [az batch job show](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_show) | Hiermee worden details van een opgegeven Batch-taak opgehaald.  |
| [az batch task show](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_show) | Haalt de details van een taak op uit de opgegeven Batch-job.  |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.