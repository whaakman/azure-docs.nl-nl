---
title: "Maken van een functie in Azure die is geïmplementeerd vanuit Visual Studio Team Services | Microsoft Docs"
description: Een functie-App maken en implementeren van de functiecode van Visual Studio Team Services
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: bf9428f23e851bae3485ec3d724dfb9ccd2af4c1
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-visual-studio-team-services"></a>Maken van een functie in Azure die vanuit Visual Studio Team Services is geïmplementeerd

Dit onderwerp leest u hoe u met Azure Functions maakt een [zonder server](https://azure.microsoft.com/overview/serverless-computing/) functie-app gebruikt de [verbruik plan](../functions-scale.md#consumption-plan). De functie-app een container voor uw functies is, wordt continu uit een opslagplaats voor Visual Studio Team Services (VSTS) geïmplementeerd. Als u dit onderwerp, moet u het volgende hebben:

* Een opslagplaats VSTS die uw functie app-project bevat en waarvoor u beheerdersmachtigingen hebt.
* Een [persoonlijke toegangstoken (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) voor toegang tot uw opslagplaats VSTS.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u in plaats daarvan de Azure CLI lokaal, moet u het installeren en gebruiken van versie 2.0 of hoger. Om te bepalen van de versie van Azure CLI, voert `az --version`. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit voorbeeld maakt u een Azure-functie-app en functiecode van Visual Studio Team Services implementeert.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, storage-account, functie-app en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. |
| [AZ functionapp maken](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [AZ appservice webconfiguratie bronbeheer](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Een functie-app koppelt aan een Git of volgt opslagplaats. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).
