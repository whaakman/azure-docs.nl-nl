---
title: 'Azure PowerShell-voorbeeldscript: Een app terugzetten vanuit een back-up in een ander abonnement | Microsoft Docs'
description: 'Azure PowerShell-voorbeeldscript: Een web-app terugzetten vanuit een back-up in een ander abonnement'
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 9e27585d44214a2826ca2c4ac705eb6bb803344a
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53586227"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription-using-powershell"></a>Een web-app terugzetten vanuit een back-up in een ander abonnement met behulp van PowerShell

Met dit voorbeeldscript haalt u een eerder voltooide back-up van een bestaande web-app op en zet u deze terug naar een web-app in een ander abonnement. 

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Connect-AzureRmAccount` uit om verbinding te maken met Azure. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Als u de web-app niet meer nodig hebt, gebruikt u de volgende opdracht om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) | Hiermee voegt u een geverifieerd account om te gebruiken voor aanvragen van Azure Resource Manager-cmdlets.  |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Hiermee vraagt u een lijst met back-ups op voor een web-app. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een web-app |
| [Restore-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Hiermee zet u een web-app terug vanuit een eerder voltooide back-up. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure App Service Web Apps vindt u in de [voorbeelden van Azure PowerShell](../samples-powershell.md).
