---
title: Azure PowerShell-Script voorbeeld - Maak een Back-up van een web-app | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Maak een Back-up van een web-app
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 5984a7707552b2740b48e3c9da40a5e96a3a279b
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="back-up-a-web-app"></a>Back-up van een web-app

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en vervolgens maakt een eenmalige back-up voor. 

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure. 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/app-service/backup-onetime/backup-onetime.ps1?highlight=1-5 "Back up a web app")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Maakt een opslagaccount. |
| [Nieuwe AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer) | Hiermee maakt u een Azure storage-container. |
| [Nieuwe AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) | Genereert een SAS-token voor een Azure storage-container.  |
| [Nieuwe AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Hiermee maakt u een App Service-abonnement. |
| [Nieuwe AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een web-app. |
| [Nieuwe AzureRmWebAppBackup](/powershell/module/azurerm.websites/new-azurermwebappbackup) | Maakt een back-up voor een web-app. |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Hiermee wordt een lijst met back-ups voor een web-app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure App Service Web Apps kunnen worden gevonden in de [voorbeelden van Azure PowerShell](../app-service-powershell-samples.md).
