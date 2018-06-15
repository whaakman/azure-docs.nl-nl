---
title: Azure CLI-voorbeeldscript - Containers verwijderen op basis van voorvoegsel | Microsoft Docs
description: Verwijder Azure Storage-blobcontainers op basis van het voorvoegsel van een containernaam.
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
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: 9e93dc14a4729011f74c5eafe94528608b89116f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29848284"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Containers verwijderen op basis van het containernaamvoorvoegsel

Met dit script worden eerst een aantal voorbeeldcontainers in Azure Blob Storage gemaakt, en vervolgens enkele containers verwijderd op basis van het voorvoegsel van de containernaam.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, de resterende containers en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt containers te verwijderen op basis van het voorvoegsel van de containernaam. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Hiermee maakt u een Azure Storage-account in de opgegeven resourcegroep. |
| [az storage container create](/cli/azure/storage/container#az_storage_container_create) | Hiermee maakt u een container in Azure Blob Storage. |
| [az storage container list](/cli/azure/storage/container#az_storage_container_list) | Hiermee maakt u een lijst van de containers in een Azure Storage-account. |
| [az storage container delete](/cli/azure/storage/container#az_storage_container_delete) | Hiermee verwijdert u containers in een Azure Storage-account. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer CLI-voorbeeldscripts voor Storage vindt u in de [Azure CLI-voorbeelden voor Azure Storage](../blobs/storage-samples-blobs-cli.md).
