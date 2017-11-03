---
title: 'Azure PowerShell-Script voorbeeld: een web-app maken met continue implementatie van GitHub | Microsoft Docs'
description: 'Azure PowerShell-Script voorbeeld: een web-app maken met continue implementatie van GitHub'
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f0574c8c549c45682206624ffe38a4a3919cb1c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Een WebApp maken met continue implementatie van GitHub

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en vervolgens stelt continue implementatie van een GitHub-opslagplaats. Zie voor de implementatie van GitHub zonder continue implementatie [een web-app maken en implementeren van code vanuit GitHub](app-service-powershell-deploy-github.md).

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure. Controleer ook of:

- Een verbinding met Azure is gemaakt met behulp van de `az login` opdracht.
- De code van de toepassing is in een openbare of particuliere GitHub-opslagplaats waarvan u eigenaar.
- U hebt [een toegangstoken gemaakt in uw GitHub-account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

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
| [Nieuwe AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Hiermee maakt u een App Service-abonnement. |
| [Nieuwe AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een web-app. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Hiermee wijzigt u een resource in een resourcegroep. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure App Service Web Apps kunnen worden gevonden in de [voorbeelden van Azure PowerShell](../app-service-powershell-samples.md).
