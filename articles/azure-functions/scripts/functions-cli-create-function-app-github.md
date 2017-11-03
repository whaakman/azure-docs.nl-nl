---
title: Een functie-App maken en implementeren van functiecode vanuit GitHub | Microsoft Docs
description: Azure CLI-voorbeeldscript - een functie-App maken en implementeren van functiecode vanuit GitHub
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 39e26ba6c3ae0927fbf6c62af0bd3b48d16657ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Een functie-app maken en implementeren van functiecode vanuit GitHub

Dit voorbeeldscript maakt een functie-app met de [verbruik plan](../functions-scale.md#consumption-plan) met de bijbehorende resources en de functiecode uit een openbare GitHub-opslagplaats (zonder continue implementatie) implementeert. Lees voor continue levering van functiecode vanuit GitHub, [maken van een functie-app en continu implementeren vanuit GitHub](functions-cli-create-function-app-github-continuous.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit voorbeeld maakt u een Azure-functie-app en implementeert functiecode vanuit GitHub.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht. Dit script maakt gebruik van de volgende opdrachten:

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. |
| [AZ functionapp maken](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) | Hiermee maakt u een Azure-functie-app. |
| [AZ appservice webconfiguratie bronbeheer](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Een functie-app koppelt aan een Git of volgt opslagplaats. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).
