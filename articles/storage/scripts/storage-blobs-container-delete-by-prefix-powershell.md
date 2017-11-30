---
title: Azure PowerShell-Script voorbeeld - containers verwijderen met het voorvoegsel | Microsoft Docs
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
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: 629189b9dbe2327763d364abc95f49539a312c53
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Verwijderen van containers op basis van het voorvoegsel van container

Dit script wordt verwijderd van containers in Azure Blob-opslag op basis van een voorvoegsel in de containernaam van de.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om te verwijderen van de resourcegroep, resterende containers, en alle gerelateerde resources.

```powershell
Remove-AzureRmResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Script uitleg

Dit script wordt de volgende opdrachten om te verwijderen van containers op basis van het voorvoegsel van container. Elk item in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Get AzureRmStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | Hiermee haalt u een opgegeven opslagaccount of alle van de Storage-accounts in een resourcegroep of het abonnement. |
| [Get-AzureStorageContainer](/powershell/module/azure.storage/get-azurestoragecontainer) | Geeft een lijst van de storage-containers die zijn gekoppeld aan een opslagaccount. |
| [Verwijder AzureStorageContainer](/powershell/module/azure.storage/remove-azurestoragecontainer) | Hiermee verwijdert u de opgegeven storage-container. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra opslagruimte PowerShell-script kunnen worden gevonden in [voorbeelden van PowerShell voor Azure-blobopslag](../blobs/storage-samples-blobs-powershell.md).
