---
title: Azure PowerShell-voorbeeldscript - verkeer routeren voor hoge beschikbaarheid van toepassingen | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - verkeer routeren voor hoge beschikbaarheid van toepassingen
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
ms.openlocfilehash: 4991be7ecb252001245a0bcde8551d8432b0794b
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998498"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-powershell"></a>Verkeer routeren voor hoge beschikbaarheid van toepassingen met behulp van Azure PowerShell

Dit script maakt u een resourcegroep, twee app service-abonnementen, twee web-apps, een traffic manager-profiel en twee traffic manager-eindpunten. Traffic Manager zorgt ervoor dat verkeer van de toepassing in één regio als de primaire regio, en naar de secundaire regio als de toepassing in de primaire regio niet beschikbaar is. Voordat u het script uitvoert, moet u de waarden MyWebApp, MyWebAppL1 en MyWebAppL2 op unieke waarden wijzigen in Azure. Nadat het script is uitgevoerd, kunt u de app in de primaire regio met de URL-mywebapp.trafficmanager.net openen.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure) en voer vervolgens `Connect-AzAccount` uit om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup1
Remove-AzResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, web-app, Traffic Manager-profiel en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Hiermee maakt u een App Service-plan. Dit is vergelijkbaar met een serverfarm voor uw Azure-web-app. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Hiermee maakt u een Azure-web-app in de App Service-plan. |
| [Set-AzResource](/powershell/module/az.resources/new-azresource) | Hiermee maakt u een Azure-web-app in de App Service-plan. |
| [New-AzTrafficManagerProfile](/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile) | Hiermee maakt u een Azure Traffic Manager-profiel. |
| [New-AzTrafficManagerEndpoint](/powershell/module/az.trafficmanager/new-aztrafficmanagerendpoint) | Een eindpunt toevoegen aan een Azure Traffic Manager-profiel. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende PowerShell-voorbeeldscripts voor netwerken vindt u in de [Documentatie met een overzicht van Azure-netwerken](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
