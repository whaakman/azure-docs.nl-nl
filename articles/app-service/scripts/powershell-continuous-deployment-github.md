---
title: 'Voorbeeld van Azure PowerShell-script: Een app maken met continue implementatie van GitHub | Microsoft Docs'
description: Voorbeeld van PowerShell-script - Een web-app maken met continue implementatie vanuit GitHub
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 7cd0366ff350995a1edf13476b4ad671d3bd3d3d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114328"
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Een web-app maken met continue implementatie vanuit GitHub

In dit voorbeeldscript wordt een web-app gemaakt in App Service met de bijbehorende resources en wordt er vervolgens continue implementatie vanuit een GitHub-opslagplaats ingesteld. Zie [Een web-app maken en code implementeren vanuit GitHub](powershell-deploy-github.md) voor de implementatie van GitHub zonder continue implementatie.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Connect-AzAccount` uit om verbinding te maken met Azure. Controleer ook of:

- Een verbinding met Azure is gemaakt met behulp van de opdracht `az login`.
- De code van de toepassing zich bevindt in een openbare of particuliere GitHub-opslagplaats waarvan u eigenaar bent.
- U [een toegangstoken hebt gemaakt in uw GitHub-account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Nadat het voorbeeldscript is uitgevoerd, kan de volgende opdracht worden gebruikt om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Hiermee maakt u een App Service-plan. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Hiermee maakt u een webtoepassing. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Hiermee wijzigt u een resource in een resourcegroep. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure App Service Web Apps vindt u in de [voorbeelden van Azure PowerShell](../samples-powershell.md).
