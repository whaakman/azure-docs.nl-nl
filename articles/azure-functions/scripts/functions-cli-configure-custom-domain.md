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
ms.date: 07/04/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 0650fffeb54ebc4390c82fb2711d7c89e0ac4572
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989415"
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Een aangepast domein toewijzen aan een functie-app

Met dit voorbeeldscript maakt u een functie-app in een App Service-abonnement en wijst u deze vervolgens toe aan een aangepast domein dat u opgeeft. Wanneer een functie-app wordt gehost in een [App Service-plan](../functions-scale.md#app-service-plan), kunt u een aangepast domein toewijzen met een CNAME of een A-record. Voor functie-apps in een [verbruiksabonnement](../functions-scale.md#consumption-plan) wordt alleen de optie CNAME ondersteund. Met dit voorbeeld maakt u een App Service-abonnement en hebt u een A-record nodig om het domein toe te wijzen. 

Om dit voorbeeldscript te kunnen uitvoeren, moet u al een A-record in uw aangepaste domein hebben geconfigureerd die naar de standaarddomeinnaam van uw web-app wijst. Zie de [instructies van Aangepast domein toewijzen voor Azure App Service](https://aka.ms/appservicecustomdns) voor meer informatie. 

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
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Hiermee maakt u een opslagaccount dat vereist is voor de functie-app. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az-appservice-plan-create) | Hiermee maakt u een App Service-abonnement dat vereist is voor het toewijzen van een aangepast domein. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Hiermee maakt u een functie-app in het App Service-abonnement. |
| [az functionapp config hostname add](https://docs.microsoft.com/cli/azure/functionapp/config/hostname#az-functionapp-config-hostname-add) | Hiermee wijst u een aangepast domein toe aan een functie-app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Extra voorbeelden van CLI-scripts van Functions vindt u in de [documentatie van Azure Functions](../functions-cli-samples.md).
