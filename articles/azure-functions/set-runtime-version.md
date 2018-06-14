---
title: Het doel van Azure Functions-runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Informatie over het opgeven van de runtimeversie van een functie-app gehost in Azure.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 6fc84642050f4b7acfa2e3c5b4518135d6a97171
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843388"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Het doel van Azure Functions-runtime-versies

Een functie-app wordt uitgevoerd op een specifieke versie van de Azure Functions-runtime. Er zijn twee primaire versies: [1.x en 2.x](functions-versions.md). In dit artikel wordt uitgelegd hoe het configureren van een functie-app in Azure worden uitgevoerd op de versie die u kiest. Zie voor meer informatie over het configureren van een lokale ontwikkelingsomgeving voor een specifieke versie [Code en test Azure Functions lokaal](functions-run-local.md).

>[!IMPORTANT]   
> Azure Functions-runtime 2.0 is een Preview-versie en op dit moment niet alle functies van Azure Functions worden ondersteund. Zie voor meer informatie [versies-overzicht van Azure Functions-runtime](functions-versions.md).

## <a name="automatic-and-manual-version-updates"></a>Automatische en handmatige versie-updates

Functies kunt u het doel van een specifieke versie van de runtime met behulp van de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling in een functie-app. De functie-app wordt op de opgegeven primaire versie bewaard totdat u expliciet wilt verplaatsen naar een nieuwe versie.

Als u alleen de primaire versie ('~ 1 voor 1.x) of 'bèta' voor 2.x opgeeft, wordt de functie-app automatisch bijgewerkt naar de nieuwe secundaire versies van de runtime wanneer deze beschikbaar. Nieuwe secundaire versies komen niet leiden tot grote wijzigingen. Als u een secundaire versie (bijvoorbeeld ' 1.0.11360') opgeeft, wordt de functie-app voor die versie bewaard totdat u deze expliciet wijzigt. 

Wanneer een nieuwe versie algemeen beschikbaar is, geeft u een opdrachtprompt in de portal de kans voor die versie omhoog verplaatsen. Nadat u hebt verplaatst naar een nieuwe versie, kunt u altijd gebruiken de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling om terug te gaan naar een eerdere versie.

Een wijziging in de runtimeversie zorgt ervoor dat een functie-app te starten.

De waarden die u kunt instellen in de `FUNCTIONS_EXTENSION_VERSION` app instellen voor het inschakelen van automatische updates is momenteel '~ 1' voor de 1.x-runtime en "bètaversie" voor 2.x.

## <a name="view-the-current-runtime-version"></a>De huidige runtimeversie weergeven

Gebruik de volgende procedure om de runtimeversie die momenteel wordt gebruikt door een functie-app weer te geven. 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de functie-app en klikt u onder **functies geconfigureerd**, kies **werken app-instellingen**. 

    ![Selecteer de functie app-instellingen](./media/functions-versions/add-update-app-setting.png)

2. In de **werken app-instellingen** tabblad, zoek de **runtimeversie**. Noteer de specifieke runtimeversie en de aangevraagde primaire versie. In het onderstaande voorbeeld de functies\_EXTENSIE\_versie app-instelling is ingesteld op `~1`.
 
   ![Selecteer de functie app-instellingen](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Doel een versie met behulp van de portal

Als u een versie dan de huidige primaire versie of versie 2.0 wilt, stelt de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de functie-app en klikt u onder **functies geconfigureerd**, kies **toepassingsinstellingen**.

    ![Selecteer de functie app-instellingen](./media/functions-versions/add-update-app-setting1a.png)

2. In de **toepassingsinstellingen** tabblad, zoek de `FUNCTIONS_EXTENSION_VERSION` instellen en wijzig de waarde in een geldige versie van de runtime 1.x of `beta` voor versie 2.0. Een tilde met hoofdversie betekent dat de nieuwste versie van die primaire versie (bijvoorbeeld ' ~ 1) gebruiken. 

    ![De functie app-instelling instellen](./media/functions-versions/add-update-app-setting2.png)

3. Klik op **opslaan** om op te slaan van de instelling toepassingsupdate. 

## <a name="target-a-version-using-azure-cli"></a>Doel een versie met Azure CLI

 U kunt ook instellen de `FUNCTIONS_EXTENSION_VERSION` met Azure CLI. De toepassingsinstelling in de functie-app met behulp van de Azure CLI bijwerken de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Vervang in deze code `<function_app>` met de naam van de functie-app. Vervang ook `<my_resource_group>` met de naam van de resourcegroep voor de functie-app. Vervang `<version>` met een geldige versie van de runtime 1.x of `beta` voor versie 2.0. 

U kunt uitvoeren met deze opdracht uit de [Azure Cloud Shell](../cloud-shell/overview.md) door te kiezen **Try it** in het vorige codevoorbeeld. U kunt ook de [Azure CLI lokaal](/cli/azure/install-azure-cli) deze opdracht wordt uitgevoerd na het uitvoeren van [az aanmelding](/cli/azure/reference-index#az_login) aan te melden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doel van de runtime 2.0 in uw lokale ontwikkelingsomgeving](functions-run-local.md)

> [!div class="nextstepaction"]
> [Zie de releaseopmerkingen voor runtime-versies](https://github.com/Azure/azure-webjobs-sdk-script/releases)