---
title: Een functie-app maken en functiecode implementeren vanuit GitHub | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een functie-app maken en functiecode implementeren vanuit GitHub
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 382fbe8919a031ca29a32e708b12070c584ed21f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Een functie-app maken en functiecode implementeren vanuit GitHub

Met dit voorbeeldscript maakt u een functie-app met behulp van het [verbruiksabonnement](../functions-scale.md#consumption-plan) met de bijbehorende resources, en implementeert u uw functiecode vanuit een openbare GitHub-opslagplaats (zonder continue implementatie). Lees voor continue levering van functiecode vanuit GitHub [Create a function app and continuously deploy from GitHub (Een functie-app maken en continu implementeren vanuit GitHub)](functions-cli-create-function-app-github-continuous.md).

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

In dit voorbeeld maakt u een Azure-functie-app en implementeert u functiecode vanuit GitHub.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht. In dit script worden de volgende opdrachten gebruikt:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) | Hiermee wordt een Azure Function-app gemaakt. |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Hiermee koppelt u een functie-app aan een Git- of Mercurial-opslagplaats. |

## <a name="next-steps"></a>Volgende stappen

Zie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview) voor meer informatie over de Azure CLI.

Meer voorbeeldscripts voor de Azure Functions CLI zijn te vinden in de [Azure Functions-documentatie](../functions-cli-samples.md).
