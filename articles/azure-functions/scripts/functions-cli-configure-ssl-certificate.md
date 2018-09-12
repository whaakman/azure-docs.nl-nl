---
title: 'Azure CLI-voorbeeldscript: een aangepast SSL-certificaat toewijzen aan een functie-app | Microsoft Docs'
description: 'Azure CLI-voorbeeldscript: een aangepast SSL-certificaat toewijzen aan een functie-app'
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/03/2013
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 8d8b96cec202edd1e93e9f7c04a5e508ed68a40e
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162730"
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Een aangepast SSL-certificaat koppelen aan een functie-app

Met dit voorbeeldscript wordt een functie-app in App Service gemaakt met de bijbehorende resources, waarna het SSL-certificaat van een aangepaste domeinnaam daaraan wordt gekoppeld. Voor dit voorbeeld hebt u het volgende nodig:

* Toegang tot de pagina voor DNS-configuratie van uw domeinregistrar.
* Een geldig PFX-bestand en het bijbehorende wachtwoord voor het SSL-certificaat dat u wilt uploaden en koppelen.
* Een geconfigureerde A-record in uw aangepaste domein die naar de standaarddomeinnaam van uw web-app wijst. Zie de [instructies van Aangepast domein toewijzen voor Azure App Service](https://aka.ms/appservicecustomdns) voor meer informatie.

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
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Hiermee maakt u een opslagaccount dat vereist is voor de functie-app. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az-appservice-plan-create) | Hiermee maakt u een App Service-abonnement dat vereist is voor het koppelen van SSL-certificaten. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Hiermee maakt u een functie-app in het App Service-abonnement. |
| [az functionapp config hostname add](https://docs.microsoft.com/cli/azure/functionapp/config/hostname#az-functionapp-config-hostname-add) | Hiermee wijst u een aangepast domein toe aan een functie-app. |
| [az functionapp config ssl upload](https://docs.microsoft.com/cli/azure/functionapp/config/ssl#az-functionapp-config-ssl-upload) | Hiermee wordt een SSL-certificaat naar een functie-app geüpload. |
| [az functionapp config ssl bind](https://docs.microsoft.com/cli/azure/functionapp/config/ssl#az-functionapp-config-ssl-bind) | Hiermee wordt een geüpload SSL-certificaat aan een functie-app gekoppeld. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van App Service CLI-scripts vindt u in de [documentatie van Azure App Service](../functions-cli-samples.md).
