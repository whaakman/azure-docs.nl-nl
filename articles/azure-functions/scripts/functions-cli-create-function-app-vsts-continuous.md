---
title: Een functie in Azure maken die wordt geïmplementeerd vanuit Visual Studio Team Services | Microsoft Docs
description: Een functie-app maken en functiecode implementeren vanuit Visual Studio Team Services
services: functions
keywords: ''
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 0bd2e0896758b4d9f019b0c9520c5e9e1f3afd94
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29842344"
---
# <a name="create-a-function-app-and-deploy-function-code-from-visual-studio-team-services"></a>Een functie-app maken en functiecode implementeren vanuit Visual Studio Team Services

In dit onderwerp leest u hoe u Azure Functions kunt gebruiken om een functie-app [zonder server](https://azure.microsoft.com/overview/serverless-computing/) te maken met behulp van het [verbruiksabonnement](../functions-scale.md#consumption-plan). De functie-app, een container voor uw functies, wordt continu vanuit een VSTS-opslagplaats (Visual Studio Team Services) geïmplementeerd. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

Voor het voltooien van dit onderwerp hebt u het volgende nodig:

* Een VSTS-opslagplaats die uw functie-app-project bevat en waarvoor u beheerdersmachtigingen hebt.
* Een [persoonlijk toegangstoken (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) voor toegang tot uw VSTS-opslagplaats.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de Azure CLI liever lokaal gebruikt, moet u versie 2.0 of hoger installeren en gebruiken. Voer `az --version` uit om te bepalen welke versie van Azure CLI u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

In dit voorbeeld maakt u een Azure-functie-app en implementeert u functiecode vanuit Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, opslagaccount, functie-app en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Hiermee koppelt u een functie-app aan een Git- of Mercurial-opslagplaats. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-voorbeeldscripts voor Azure Functions vindt u in de [Azure Functions-documentatie](../functions-cli-samples.md).
