---
title: Azure CLI-voorbeeldscript - Blobcontainergrootte berekenen | Microsoft Docs
description: Bereken de grootte van een container in Azure Blob Storage door de grootte van de blobs in de container op te tellen.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: c38a49e82a71a23fdf621f5ac350c4242ffc2f8f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29847104"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>De grootte van een Blob Storage-container berekenen

Met dit script wordt de grootte van een container in Azure Blob Storage berekend door de grootte van de blobs in de container op te tellen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Dit CLI-script geeft een geschatte grootte van de container en dient niet te worden gebruikt voor factureringsberekeningen.

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, container en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de grootte van de Blob Storage-container te berekenen. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az storage blob upload](/cli/azure/storage/account#az_storage_account_create) | Hiermee worden lokale bestanden geüpload naar een Azure Blob Storage-container. |
| [az storage blob list](/cli/azure/storage/account/keys#az_storage_account_keys_list) | Hiermee wordt een lijst gemaakt van de blobs in een Azure Blob Storage-container. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer CLI-voorbeeldscripts voor Storage vindt u in de [Azure CLI-voorbeelden voor Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
