---
title: 'Azure CLI-voorbeeldscript: een aangepast SSL-certificaat toewijzen aan een functie-app | Microsoft Docs'
description: 'Azure CLI-voorbeeldscript: een aangepast SSL-certificaat toewijzen aan een functie-app'
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: fd4c69036960364e12aeea5d9e5f65e7b36eff0d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843473"
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Een aangepast SSL-certificaat koppelen aan een functie-app

Met dit voorbeeldscript wordt een functie-app in App Service gemaakt met de bijbehorende resources, waarna het SSL-certificaat van een aangepaste domeinnaam daaraan wordt gekoppeld. Voor dit voorbeeld hebt u het volgende nodig:

* Toegang tot de pagina voor DNS-configuratie van uw domeinregistrar.
* Een geldig .PFX-bestand en het bijbehorende wachtwoord voor het SSL-certificaat dat u wilt uploaden en koppelen.

Als u een SSL-certificaat wilt koppelen, moet de functie-app worden gemaakt in een App Service-abonnement en niet in een verbruiksabonnement.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u liever de Azure CLI installeert en lokaal gebruikt, moet u versie 2.0 of hoger van Azure CLI gebruiken. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement dat vereist is voor het koppelen van SSL-certificaten. |
| [az functionapp create]() | Hiermee maakt u een functie-app. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Hiermee wijst u een aangepast domein toe aan de functie-app. |
| [az appservice web config ssl upload](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_upload) | Hiermee wordt een SSL-certificaat naar een functie-app geüpload. |
| [az appservice web config ssl bind](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_bind) | Hiermee wordt een geüpload SSL-certificaat aan een functie-app gekoppeld. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van App Service CLI-scripts vindt u in de [documentatie van Azure App Service]().
