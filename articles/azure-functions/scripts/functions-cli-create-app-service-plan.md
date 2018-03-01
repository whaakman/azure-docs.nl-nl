---
title: Azure CLI-voorbeeldscript - Een functie-app maken in een App Service-abonnement | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een functie-app maken in een App Service-abonnement
services: functions
documentationcenter: functions
author: syntaxc4
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/22/2018
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c035eabe966eed97ce8c1df1e4ffba327f07ccac
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>Een functie-app maken in een App Service-abonnement

Met dit voorbeeldscript voor Azure Functions maakt u een functie-app die een container vormt voor uw functies. De functie-app die wordt gemaakt, maakt gebruik van een specifiek App Service-abonnement, wat betekent dat uw serverbronnen altijd zijn ingeschakeld.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

Met dit script maakt u een Azure Function-app met behulp van een toegewezen [App Service-abonnement](../functions-scale.md#app-service-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht. In dit script worden de volgende opdrachten gebruikt:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Hiermee maakt u een Azure-opslagaccount. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appserviceplan#az_appserviceplan_create) | Hiermee maakt u een App Service-abonnement. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_delete) | Hiermee maakt u een Azure Function-app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview) voor meer informatie over de Azure CLI.

Aanvullende CLI-voorbeeldscripts voor Azure Functions vindt u in de [Azure Functions-documentatie](../functions-cli-samples.md).
