---
title: Azure PowerShell-Script voorbeeld - routeverkeer voor hoge beschikbaarheid van toepassingen | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - routeverkeer voor hoge beschikbaarheid van toepassingen
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 2f0ac4fd1779661aab04bafb217e64af5d619a2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Verkeer leiden voor hoge beschikbaarheid van toepassingen

Dit script maakt een resourcegroep, twee app-serviceabonnementen, twee web-apps, een traffic manager-profiel en twee traffic manager-eindpunten. Traffic Manager zorgt ervoor dat verkeer van de toepassing in een regio als de primaire regio en de secundaire regio als de toepassing in de primaire regio niet beschikbaar is. Voordat het script wordt uitgevoerd, moet u de waarden MyWebApp, MyWebAppL1 en MyWebAppL2 op unieke waarden in Azure. Nadat het script is uitgevoerd, kunt u de app in de primaire regio met de URL-mywebapp.trafficmanager.net openen.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, web-app, traffic manager-profiel en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Hiermee maakt u een App Service-abonnement. Dit is vergelijkbaar met een serverfarm voor uw Azure-web-app. |
| [Nieuwe AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een Azure-web-app in App Service-abonnement. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Hiermee maakt u een Azure-web-app in App Service-abonnement. |
| [Nieuwe AzureRmTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Een Azure Traffic Manager-profiel maakt. |
| [Nieuwe AzureRmTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Voegt een eindpunt toe aan een Azure Traffic Manager-profiel. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).