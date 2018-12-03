---
title: 'Azure PowerShell-voorbeeldscript: Een web-app terugzetten vanuit een back-up in een ander abonnement | Microsoft Docs'
description: 'Azure PowerShell-voorbeeldscript: Een web-app terugzetten vanuit een back-up in een ander abonnement'
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 7ed593e408b57246ef155ff8e36f054aacb2e063
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291554"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription"></a>Een web-app terugzetten vanuit een back-up in een ander abonnement

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

Meer voorbeelden voor Azure Powershell voor Azure App Service Web Apps vindt u in de [voorbeelden van Azure PowerShell](../app-service-powershell-samples.md).
