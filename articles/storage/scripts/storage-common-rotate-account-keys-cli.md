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
ms.openlocfilehash: ac58886225221677aa003833167ff58cd578255d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693918"
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
| [az group create](/cli/azure/group) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](/cli/azure/storage/account) | Hiermee maakt u een Azure Storage-account in de opgegeven resourcegroep. |
| [az storage account keys list](/cli/azure/storage/account/keys) | Hiermee geeft u de toegangssleutels voor het opgegeven opslagaccount weer. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | Hiermee wordt de primaire of secundaire toegangssleutel van het opslagaccount opnieuw gegenereerd. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer CLI-voorbeeldscripts voor Storage vindt u in de [Azure CLI-voorbeelden voor Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
