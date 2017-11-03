---
title: Azure CLI-voorbeeldscript - Bind een aangepaste SSL-certificaat voor een web-app | Microsoft Docs
description: Azure CLI-voorbeeldscript - Bind een aangepaste SSL-certificaat voor een web-app
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ab4e211bc2c3f7e99ab0246e7abba57795318245
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>Een aangepaste SSL-certificaat binden aan een web-app

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en vervolgens koppelt het SSL-certificaat van een aangepaste domeinnaam aan deze. Voor dit voorbeeld hebt u het volgende nodig:

* Toegang tot de pagina voor DNS-configuratie van uw domeinregistrar.
* Een geldig. PFX-bestand en het bijbehorende wachtwoord voor het SSL-certificaat dat u wilt uploaden en binden.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ appservice-abonnement maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. |
| [AZ webapp maken](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Hiermee maakt u een Azure-web-app. |
| [AZ webapp config hostnaam toevoegen](https://docs.microsoft.com/cli/azure/webapp/config/hostname#az_webapp_config_hostname_add) | Een aangepast domein wordt toegewezen aan een web-app. |
| [AZ webapp config ssl uploaden](https://docs.microsoft.com/cli/azure/webapp/config/ssl#az_webapp_config_ssl_upload) | Een SSL-certificaat naar een web-app geüpload. |
| [AZ webapp config ssl-binding](https://docs.microsoft.com/cli/azure/webapp/config/ssl#az_webapp_config_ssl_bind) | Een geüploade SSL-certificaat koppelt aan een web-app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra-App Service CLI scriptvoorbeelden vindt u in de [Azure App Service-documentatie](../app-service-cli-samples.md).
