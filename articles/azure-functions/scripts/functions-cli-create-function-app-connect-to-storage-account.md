---
title: Maken van een Azure-functie die is verbonden met een Azure Storage | Microsoft Docs
description: 'Azure CLI-Script voorbeeld: een Azure-functie die is verbonden met een Azure-opslag maken'
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 363a3fd1c80538495658720274840b921baa8675
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a>Functie-App integreren met Azure Storage-Account

Dit voorbeeldscript wordt gemaakt voor een functie-App en Storage-Account.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal gebruikt, zorg ervoor dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit voorbeeld wordt een Azure-functie-app gemaakt en wordt de verbindingsreeks voor opslag toegevoegd aan een app-instelling.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript uitvoeren van de volgende opdracht om het verwijderen van de resourcegroep en alle gerelateerde resources:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ aanmelding](https://docs.microsoft.com/cli/azure/#login) | Aanmelden bij Azure. |
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Een resourcegroep maken met de locatie |
| [AZ storage-account maken](https://docs.microsoft.com/cli/azure/storage/account) | Een opslagaccount maken |
| [AZ functionapp maken](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Een nieuwe functie-app maken |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Opruimen |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).
