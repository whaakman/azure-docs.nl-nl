---
title: Azure PowerShell-Script voorbeeld - een back-up voor een web-app verwijderen | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - een back-up voor een web-app verwijderen
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: ebcadb49-755d-4202-a5eb-f211827a9168
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f4204cbb4aefe590b87d0a72675823321f280f33
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="delete-a-backup-for-a-web-app"></a>Een back-up voor een web-app verwijderen

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en vervolgens maakt een eenmalige back-up voor. 

Als u wilt dit script uitvoert, moet u een bestaande back-up voor een web-app. Als u wilt maken, Zie [back-up van een web-app](app-service-powershell-backup-onetime.md) of [maken van een geplande back-up voor een web-app](app-service-powershell-backup-scheduled.md).

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/app-service/backup-delete/backup-delete.ps1?highlight=1-2,11 "Delete a backup for a web app")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Hiermee wordt een lijst met back-ups voor een web-app. |
| [Verwijder AzureRmWebAppBackup](/powershell/module/azurerm.websites/remove-azurermwebappbackup) | Hiermee verwijdert u de opgegeven back-up van een web-app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure App Service Web Apps kunnen worden gevonden in de [voorbeelden van Azure PowerShell](../app-service-powershell-samples.md).
