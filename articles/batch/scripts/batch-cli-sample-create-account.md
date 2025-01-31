---
title: Azure CLI-voorbeeldscript - Batch-account maken - Batch-service | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een batch-account maken in Batch-servicemodus | Microsoft Docs
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: lahugh
ms.openlocfilehash: 67504d9597eb68faceb67a3e5a1d4d7abc7079c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66127412"
---
# <a name="cli-example-create-a-batch-account-in-batch-service-mode"></a>CLI-voorbeeld: Een Batch-account maken in Batch-servicemodus

Dit script maakt een Azure Batch-account in de Batch-servicemodus en laat zien hoe u verschillende eigenschappen van het account kunt opvragen of bijwerken. Wanneer u een Batch-account maakt in de standaard Batch-servicemodus, worden de rekenknooppunten ervan intern toegewezen door de Batch-service. Toegewezen rekenknooppunten hebben een afzonderlijk vCPU (kern) quotum en het account kan worden geauthenticeerd via gedeelde sleutelgegevens of een Azure Active Directory-token.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0.20 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Hiermee wordt de Batch-account gemaakt. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Hiermee maakt u een opslagaccount. |
| [az batch account set](/cli/azure/batch/account#az-batch-account-set) | Hiermee worden eigenschappen van het Batch-account bijgewerkt.  |
| [az batch account show](/cli/azure/batch/account#az-batch-account-show) | Hiermee worden details van het opgegeven Batch-account opgehaald.  |
| [az batch account keys list](/cli/azure/batch/account/keys#az-batch-account-keys-list) | Hiermee worden de toegangssleutels van het opgegeven Batch-account opgehaald.  |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Hiermee wordt authenticatie uitgevoerd met het opgegeven Batch-account voor verdere interactie met de CLI.  |
| [az group delete](/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.
