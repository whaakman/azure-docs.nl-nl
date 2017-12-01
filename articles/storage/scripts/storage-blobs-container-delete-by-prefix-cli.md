---
title: Azure CLI-voorbeeldscript - containers verwijderen met het voorvoegsel | Microsoft Docs
description: Azure Storage-blob-containers op basis van het voorvoegsel van een container verwijderen.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: d14195abf1c17d11e259ed9edb5112626b063112
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Verwijderen van containers op basis van het voorvoegsel van container

Dit script eerst maakt een paar voorbeeld containers in Azure Blob-opslag en vervolgens verwijdert enkele van de containers op basis van een voorvoegsel in de containernaam van de.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om te verwijderen van de resourcegroep, resterende containers, en alle gerelateerde resources.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script wordt de volgende opdrachten om te verwijderen van containers op basis van het voorvoegsel van container. Elk item in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](/cli/azure/group#create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](/cli/azure/storage/account#create) | Maakt een Azure Storage-account in de opgegeven resourcegroep. |
| [AZ storage-container maken](/cli/azure/storage/container#create) | Maakt een container in Azure Blob-opslag. |
| [lijst met AZ storage-container](/cli/azure/storage/container#list) | Geeft een lijst van de containers in een Azure Storage-account. |
| [AZ opslag container verwijderen](/cli/azure/storage/container#delete) | Hiermee verwijdert u containers in een Azure Storage-account. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](/cli/azure/overview).

Extra opslagruimte CLI scriptvoorbeelden vindt u in de [voorbeelden van Azure CLI voor Azure Storage](../blobs/storage-samples-blobs-cli.md).
