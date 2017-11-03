---
title: Azure CLI-voorbeeldscript - toegang van draaien opslagaccountsleutels | Microsoft Docs
description: Een Azure Storage-account maken en vervolgens ophalen en de toegangssleutels van account draaien.
services: storage
documentationcenter: na
author: mmacy
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
ms.author: marsma
ms.openlocfilehash: 1b59144e0426b50c2ac49acbd7914d975ff037ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Een opslagaccount maken en de toegangssleutels van account draaien

Dit script maakt een Azure Storage-account, toegangstoetsen voor het nieuwe opslagaccount wordt weergegeven en wordt verlengd (draait) de sleutels.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, storage-account en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten om de storage-account maken, ophalen en de toegangstoetsen draaien. Elk item in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](/cli/azure/group#create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](/cli/azure/storage/account#create) | Maakt een Azure Storage-account in de opgegeven resourcegroep. |
| [lijst met opslagaccounts die sleutels AZ](/cli/azure/storage/account/keys#list) | Geeft de opslagaccountsleutels voor toegang voor het opgegeven account. |
| [de opslagaccountsleutels AZ vernieuwen](/cli/azure/storage/account/keys#renew) | Opnieuw genereren toegangssleutel voor de primaire of secundaire opslagaccount. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](/cli/azure/overview).

Extra opslagruimte CLI scriptvoorbeelden vindt u in de [voorbeelden van Azure CLI voor Azure-blobopslag](../blobs/storage-samples-blobs-cli.md).
