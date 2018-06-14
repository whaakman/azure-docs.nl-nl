---
title: Azure CLI-voorbeeldscript - Opslagaccountsleutels draaien | Microsoft Docs
description: Maak een Azure Storage-account en haal de toegangssleutels van het account op en draai ze.
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
ms.openlocfilehash: 160509a5a82b71b281d57d97e103bb4190605b7c
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29847808"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Aen Azure Storage-account maken en de accounttoegangssleutels draaien

Met dit script wordt een Azure Storage-account gemaakt, worden de toegangssleutels van het nieuwe opslagaccount weergegeven en worden de sleutels vernieuwd (gedraaid).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, het opslagaccount en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om het opslagaccount te maken en de toegangssleutels ervan op te halen en te draaien. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Hiermee maakt u een Azure Storage-account in de opgegeven resourcegroep. |
| [az storage account keys list](/cli/azure/storage/account/keys#az_storage_account_keys_list) | Hiermee geeft u de toegangssleutels voor het opgegeven opslagaccount weer. |
| [az storage account keys renew](/cli/azure/storage/account/keys#az_storage_account_keys_renew) | Hiermee wordt de primaire of secundaire toegangssleutel van het opslagaccount opnieuw gegenereerd. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer CLI-voorbeeldscripts voor Storage vindt u in de [Azure CLI-voorbeelden voor Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
