---
title: Azure PowerShell-voorbeeldscript - routeverkeer voor hoge beschikbaarheid van toepassingen | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - routeverkeer voor hoge beschikbaarheid van toepassingen
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: kumud
ms.openlocfilehash: 13c24c31606d99f27ed2607fec71b381160624dd
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
ms.locfileid: "32313053"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-powershell"></a>Verkeer leiden voor hoge beschikbaarheid van toepassingen met Azure PowerShell

Dit script maakt een resourcegroep, twee app-serviceabonnementen, twee web-apps, een traffic manager-profiel en twee traffic manager-eindpunten. Traffic Manager zorgt ervoor dat verkeer van de toepassing in een regio als de primaire regio en de secundaire regio als de toepassing in de primaire regio niet beschikbaar is. Voordat het script wordt uitgevoerd, moet u de waarden MyWebApp, MyWebAppL1 en MyWebAppL2 op unieke waarden in Azure. Nadat het script is uitgevoerd, kunt u de app in de primaire regio met de URL-mywebapp.trafficmanager.net openen.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) en voer vervolgens `Connect-AzureRmAccount` uit om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, web-app, Traffic Manager-profiel en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Hiermee maakt u een App Service-plan. Dit is vergelijkbaar met een serverfarm voor uw Azure-web-app. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een Azure-web-app in App Service-abonnement. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Hiermee maakt u een Azure-web-app in App Service-abonnement. |
| [Nieuwe AzureRmTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Hiermee maakt u een Azure Traffic Manager-profiel. |
| [Nieuwe AzureRmTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Voegt een eindpunt toe aan een Azure Traffic Manager-profiel. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
