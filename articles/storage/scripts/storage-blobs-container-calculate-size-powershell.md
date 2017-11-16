---
title: Azure PowerShell-Script steekproef - berekenen blob-container grootte | Microsoft Docs
description: De grootte van een container in Azure Blob-opslag berekenen door de grootte van elk van de blobs totaal te berekenen.
services: storage
documentationcenter: na
author: tamram
manager: jeconnoc
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 11/07/2017
ms.author: tamram
ms.openlocfilehash: f6f421e780bfbb7922a4b11f758330f2a9a0b84b
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>De grootte van een Blob storage-container berekenen

Dit script wordt de grootte van een container in Azure Blob storage berekend door het totaal te berekenen van de grootte van de blobs in de container.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Deze PowerShell-script bevat een geschatte grootte voor de container en mag niet worden gebruikt voor facturering berekeningen. Zie voor een script dat berekent de grootte van de container voor facturatie, [berekenen van de grootte van een Blob storage-container voor factureringsdoeleinden](../scripts/storage-blobs-container-calculate-billing-size-powershell.md). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size.ps1 "Calculate container size")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, container en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de grootte van de Blob storage-container te berekenen. Elk item in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Get AzureRmStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | Hiermee haalt u een opgegeven opslagaccount of alle van de Storage-accounts in een resourcegroep of het abonnement. |
| [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) | Een lijst met blobs in een container. ||

## <a name="next-steps"></a>Volgende stappen

Zie voor een script dat berekent de grootte van de container voor facturatie, [berekenen van de grootte van een Blob storage-container voor factureringsdoeleinden](../scripts/storage-blobs-container-calculate-billing-size-powershell.md).

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra opslagruimte PowerShell-script kunnen worden gevonden in [voorbeelden van PowerShell voor Azure Storage](../blobs/storage-samples-blobs-powershell.md).
