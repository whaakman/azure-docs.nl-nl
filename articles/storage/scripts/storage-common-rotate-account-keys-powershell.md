---
title: Azure PowerShell-Script voorbeeld - toegangssleutel voor opslagaccount draaien | Microsoft Docs
description: Een Azure Storage-account maken en vervolgens ophalen en een van de toegangssleutels van account draaien.
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
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: a7aaacf316799540a6a72b699ba8ea8bb389c8a8
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Een opslagaccount maken en de toegangssleutels van account draaien

Dit script maakt een Azure Storage-account, primaire toegangssleutel voor het nieuwe opslagaccount wordt weergegeven en wordt vernieuwd (draait) de sleutel.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, storage-account en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten om de storage-account maken, ophalen en een van de toegangssleutels draaien. Elk item in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) | Hiermee haalt alle locaties en de ondersteunde resourceproviders voor elke locatie. |
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een Azure-resourcegroep. |
| [Nieuwe AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Maakt een opslagaccount. |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Hiermee haalt u de toegangssleutels voor een Azure Storage-account. |
| [Nieuwe AzureRmStorageAccountKey](/powershell/module/azurerm.storage/new-azurermstorageaccountkey) | Genereert een toegangssleutel voor een Azure Storage-account. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra opslagruimte PowerShell-script kunnen worden gevonden in [voorbeelden van PowerShell voor Azure-blobopslag](../blobs/storage-samples-blobs-powershell.md).
