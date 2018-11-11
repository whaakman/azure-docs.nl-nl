---
title: Azure CLI-voorbeeldscript - Een Batch AI-cluster maken met behulp van een configuratiebestand | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een Batch AI-cluster maken door configuratie-instellingen op te geven in een JSON-bestand.
services: batch-ai
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.devlang: azurecli
ms.topic: sample
ms.tgt-pltfrm: multiple
ms.workload: na
ms.date: 08/16/2018
ms.author: danlep
ms.openlocfilehash: 01281c0328fab85814ff93c73f9ea25e0d4c1b08
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232797"
---
# <a name="cli-example-create-a-batch-ai-cluster-using-a-cluster-configuration-file"></a>CLI-voorbeeld: een Batch AI-cluster maken met behulp van een clusterconfiguratiebestand

Dit script laat zien hoe u instellingen voor een Batch AI-cluster opgeeft met een JSON-configuratiebestand. Gebruik deze instellingen in plaats van de bijbehorende opdrachtregelparameters voor `az batchai cluster create`. Een configuratiebestand is handig wanneer u meerdere bestandssystemen op de clusterknooppunten wilt koppelen of een identieke configuratie wilt gebruiken in verschillende clusters.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze handleiding de Azure CLI 2.0.38 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bepalen. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-config-file.sh "Create Batch AI cluster - configuration file")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdrachten om de resourcegroepen en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Hiermee maakt u een opslagaccount. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Hiermee maakt u een bestandsshare in een opslagaccount. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Hiermee maakt u een Batch AI-werkruimte. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Hiermee maakt u een Batch AI-cluster. |
| [az batchai cluster show](/cli/azure/batchai/cluster#az-batchai-cluster-show) | Hiermee geeft u informatie over een Batch AI-cluster weer. |
| [az group delete](/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.
