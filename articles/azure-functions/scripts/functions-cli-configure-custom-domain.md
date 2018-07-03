---
title: Voorbeeld van Azure CLI-script - Een aangepast domein toewijzen aan een functie-app | Microsoft Docs
description: Voorbeeld van Azure CLI-script - Een aangepast domein toewijzen aan een functie-app in Azure.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/26/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7d3fc71bc53e85fa7555dbee5ee79b3f06f27fe8
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960335"
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Een aangepast domein toewijzen aan een functie-app

Met dit voorbeeldscript wordt er een functie-app gemaakt, inclusief de bijbehorende resources, waarna `www.<yourdomain>` wordt gekoppeld aan de app. Wanneer een functie-app wordt gehost in een [App Service-plan](../functions-scale.md#app-service-plan), kunt u een aangepast domein toewijzen met een CNAME of een A-record. Voor functie-apps in een [verbruiksabonnement](../functions-scale.md#consumption-plan) wordt alleen de optie CNAME ondersteund.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u liever de Azure CLI installeert en lokaal gebruikt, moet u versie 2.0 of hoger van Azure CLI gebruiken. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Hiermee maakt u een opslagaccount dat vereist is voor de functie-app. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement dat vereist is voor het toewijzen van een aangepast domein. |
| [az functionapp create]() | Hiermee maakt u een functie-app. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Hiermee wijst u een aangepast domein toe aan een functie-app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Extra voorbeelden van CLI-scripts van Functions vindt u in de [documentatie van Azure Functions]().
