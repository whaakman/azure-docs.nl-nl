---
title: Azure CLI-Script steekproef - berekenen blob-container grootte | Microsoft Docs
description: De grootte van een container in Azure Blob-opslag berekenen door het totaal te berekenen van de grootte van de blobs in de container.
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
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: 61a553e47a642aead323a19d0724fdccc94a6282
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>De grootte van een Blob storage-container berekenen

Dit script wordt de grootte van een container in Azure Blob storage berekend door het totaal te berekenen van de grootte van de blobs in de container.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Dit script CLI biedt een geschatte grootte voor de container en mag niet worden gebruikt voor facturering berekeningen.

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, container en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de grootte van de Blob storage-container te berekenen. Elk item in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](/cli/azure/group#create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [uploaden van blob storage AZ](/cli/azure/storage/account#create) | Lokale bestanden naar een Azure Blob storage-container geüpload. |
| [lijst met blob storage AZ](/cli/azure/storage/account/keys#list) | Hier worden de blobs in een Azure Blob storage-container. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](/cli/azure/overview).

Extra opslagruimte CLI scriptvoorbeelden vindt u in de [voorbeelden van Azure CLI voor Azure-blobopslag](../blobs/storage-samples-blobs-cli.md).
