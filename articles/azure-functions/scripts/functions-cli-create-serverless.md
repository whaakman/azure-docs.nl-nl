---
title: Azure CLI-Script steekproef - maken van een functie-App voor uitvoering zonder server | Microsoft Docs
description: Azure CLI-Script steekproef - maken van een functie-App voor uitvoering zonder server
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
ms.date: 10/04/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 9436cafa775f2ad658c0c9901f7f8eae769c7052
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a>Maken van een functie-App voor uitvoering zonder server

Dit voorbeeldscript wordt gemaakt van een functie-App van Azure, dat een container voor uw functies is. De functie-App wordt gemaakt met de [verbruik plan](../functions-scale.md#consumption-plan), dit is ideaal voor workloads van gebeurtenisafhankelijke zonder server.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit script maakt een Azure-functie app met behulp van de [verbruik plan](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht. Dit script maakt gebruik van de volgende opdrachten:

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](/cli/azure/storage/account#az_storage_account_create) | Een Azure Storage-account maakt. |
| [AZ functionapp maken](/cli/azure/functionapp#az_functionapp_create) | Hiermee maakt u een functie-app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).
