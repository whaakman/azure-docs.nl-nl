---
title: Voorbeeld van Azure PowerShell-script - Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 61a295d346d589af2beea190921a1cf64596781d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats

Met dit voorbeeldscript wordt er in App Service een web-app met bijbehorende resources gemaakt. Daarna wordt de code van de web-app geïmplementeerd vanuit een lokale Git-opslagplaats.

Gebruik zo nodig de instructie in de [Azure PowerShell-handleiding](/powershell/azure/overview) om bij te werken naar de nieuwste Azure PowerShell, en voer `Connect-AzureRmAccount` uit om een verbinding te maken met Azure. Daarnaast moet de toepassingscode worden vastgelegd in een lokale Git-opslagplaats.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Nadat het voorbeeldscript is uitgevoerd, kan de volgende opdracht worden gebruikt om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een web-app met de nodig resourcegroep en App Service-groep. Wanneer de huidige map een Git-opslagplaats bevat, voeg dan ook een externe `azure` toe. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure App Service Web Apps vindt u in de [voorbeelden van Azure PowerShell](../app-service-powershell-samples.md).
