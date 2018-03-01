---
title: Een Azure-functie maken die verbinding maakt met een Azure Storage | Microsoft Docs
description: Azure CLI-scriptvoorbeeld - Een Azure-functie maken die verbinding maakt met een Azure Storage
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
ms.openlocfilehash: b4dad0bba7a147f294c5d9ce3a1a1aa8e95058f3
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-function-app-that-connects-to-an-azure-storage-account"></a>Een functie-app maken die verbinding maakt met een Azure Storage-account

Met dit Azure Functions-voorbeeldscript wordt een functie-app gemaakt en wordt de functie verbonden met een Azure Storage-account. De gemaakte app-instelling die de verbinding bevat, kan worden gebruikt met een [[opslagtrigger of binding](..\functions-bindings-storage-blob.md). 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal gebruikt, moet u ervoor zorgen dat u Azure CLI versie 2.0 of hoger gebruikt. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

In dit voorbeeld wordt een Azure-functie-app gemaakt en wordt de opslagverbindingsreeks toegevoegd aan een app-instelling.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Opschonen van implementatie

Voer nadat het voorbeeldscript is uitgevoerd de volgende opdracht uit om de resourcegroep en alle gerelateerde resources te verwijderen:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/#az_login) | Aanmelden bij Azure. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Een resourcegroep maken met locatie |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account) | Een opslagaccount maken |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Een nieuwe functie-app maken |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Opruimen |

## <a name="next-steps"></a>Volgende stappen

Zie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview) voor meer informatie over de Azure CLI.

Meer voorbeeldscripts voor de Azure Functions CLI zijn te vinden in de [Azure Functions-documentatie](../functions-cli-samples.md).
