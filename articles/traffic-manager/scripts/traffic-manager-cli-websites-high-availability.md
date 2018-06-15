---
title: Azure CLI-voorbeeldscript - routeverkeer voor hoge beschikbaarheid van toepassingen | Microsoft Docs
description: Azure CLI-voorbeeldscript - routeverkeer voor hoge beschikbaarheid van toepassingen
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: tysonn
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: kumud
ms.openlocfilehash: 82264e51fcd64615a41f5fa652c4ad58ad9dabfb
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
ms.locfileid: "32313046"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-cli"></a>Verkeer leiden voor hoge beschikbaarheid van toepassingen met Azure CLI

Dit script maakt een resourcegroep, twee app-serviceabonnementen, twee web-apps, een traffic manager-profiel en twee traffic manager-eindpunten. Traffic Manager zorgt ervoor dat verkeer van de toepassing in een regio als de primaire regio en de secundaire regio als de toepassing in de primaire regio niet beschikbaar is. Voordat het script wordt uitgevoerd, moet u de waarden MyWebApp, MyWebAppL1 en MyWebAppL2 op unieke waarden in Azure. Nadat het script is uitgevoerd, kunt u de app in de primaire regio met de URL-mywebapp.trafficmanager.net openen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Na het uitvoeren van het voorbeeldscript de volgende opdracht kan worden gebruikt voor het verwijderen van de resourcegroep, de App Service-app en alle bijbehorende resources.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, web-app, Traffic Manager-profiel en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-plan. Dit is vergelijkbaar met een serverfarm voor uw Azure-web-app. |
| [AZ appservice web maken](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_create) | Hiermee maakt u een Azure-web-app in App Service-abonnement. |
| [AZ netwerk traffic manager-profiel maken](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#az_network_traffic_manager_profile_create) | Hiermee maakt u een Azure Traffic Manager-profiel. |
| [netwerkeindpunt voor het traffic manager AZ maken](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#az_network_traffic_manager_endpoint_create) | Voegt een eindpunt toe aan een Azure Traffic Manager-profiel. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Extra-App Service CLI scriptvoorbeelden vindt u in de [documentatie Azure Networking](../cli-samples.md).
