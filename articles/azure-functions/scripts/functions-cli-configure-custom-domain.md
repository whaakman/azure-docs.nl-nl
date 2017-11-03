---
title: Azure CLI-voorbeeldscript - kaart een aangepast domein voor een functie-app | Microsoft Docs
description: 'Azure CLI - voorbeeldscript: de toewijzing van een aangepast domein in een functie-app in Azure.'
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 87b79d6222f40e3dc1306ecace51bae50b06e484
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Een aangepast domein toewijzen aan een functie-app

Dit voorbeeldscript wordt gemaakt van een functie-app met verwante resources en wijst vervolgens `www.<yourdomain>` aan. Aan een aangepast domein wilt toewijzen, moet de functie-app in App Service-abonnement en niet in een plan verbruik worden gemaakt. Azure Functions biedt alleen ondersteuning voor het toewijzen van een aangepast domein met een A-record.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en de CLI lokaal gebruiken, moet u de Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten: elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ storage-account maken](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Maakt een opslagaccount dat is vereist voor de functie-app. |
| [AZ appservice-abonnement maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement vereist als een aangepast domein wilt toewijzen. |
| [AZ functionapp maken]() | Hiermee maakt u een functie-app. |
| [AZ appservice web config hostnaam toevoegen](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Een aangepast domein wordt toegewezen aan een functie-app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Voorbeelden van aanvullende functies CLI-script kunnen worden gevonden in de [documentatie van Azure Functions]().
