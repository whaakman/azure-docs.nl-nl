---
title: Een functie-App maken en implementeren van de functiecode van Visual Studio Team Services | Microsoft Docs
description: Een functie-App maken en implementeren van de functiecode van Visual Studio Team Services
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 5851b5219b6e25a5a2b005fc3d3c3b44d98ed746
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-app-service"></a>Maak een App Service

In dit scenario wordt u informatie over het maken van een functie-app met de [verbruik plan](../functions-scale.md#consumption-plan) met de bijbehorende resources en continu implementeert uw functiecode uit de opslagplaats van een Visual Studio Team Services (VSTS). In dit voorbeeld hebt u het volgende nodig:

* Een opslagplaats VSTS met functies code, die u hebt beheerdersbevoegdheden nodig voor.
* Een [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) voor uw GitHub-account.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit voorbeeld maakt u een Azure-functie-app en functiecode van Visual Studio Team Services implementeert.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, web-app, documentdb en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. |
| [AZ functionapp maken](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [AZ appservice webconfiguratie bronbeheer](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Een functie-app koppelt aan een Git of volgt opslagplaats. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).
