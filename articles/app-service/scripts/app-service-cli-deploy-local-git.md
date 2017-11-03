---
title: Azure CLI-voorbeeldscript - web-app maken en implementeren van de code van een lokale Git-opslagplaats | Microsoft Docs
description: Azure CLI-voorbeeldscript - web-app maken en implementeren van de code van een lokale Git-opslagplaats
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 71cd1aeaa336d484d76e9a51511abcd38a878f00
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Een web-app maken en implementeren van de code van een lokale Git-opslagplaats

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en implementeert vervolgens uw web-app-code in een lokale Git-opslagplaats.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create a web app and deploy code from a local Git repository")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ appservice-abonnement maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. |
| [AZ webapp maken](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Hiermee maakt u een Azure-web-app. |
| [AZ webapp implementatiegebruiker ingesteld](https://review.docs.microsoft.com/cli/azure/webapp/deployment/user#set) | Hiermee stelt de referenties voor account-niveau implementatie voor App Service. |
| [bron in AZ webapp implementatie-config-local-git](https://review.docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | Maakt een configuratie van de bron-besturingselement voor een lokale Git-opslagplaats. |
| [AZ webapp bladeren](https://docs.microsoft.com/cli/azure/webapp#az_webapp_browse) | Een Azure-web-app in een browser openen. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra-App Service CLI scriptvoorbeelden vindt u in de [Azure App Service-documentatie](../app-service-cli-samples.md).
