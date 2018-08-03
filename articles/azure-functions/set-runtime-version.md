---
title: Het doel-Azure Functions runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Informatie over het opgeven van de runtimeversie van een functie-app die wordt gehost in Azure.
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
ms.openlocfilehash: 3efc09f93cf8b3e65d595c87d0cf25691386d6e2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434761"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Het doel-Azure Functions runtime-versies

Een functie-app wordt uitgevoerd op een specifieke versie van de Azure Functions-runtime. Er zijn twee primaire versies: [1.x en 2.x](functions-versions.md). In dit artikel wordt uitgelegd hoe u een functie-app configureren in Azure uit te voeren op de versie die u kiest. Zie voor meer informatie over het configureren van een lokale ontwikkelingsomgeving voor een specifieke versie [Code en test Azure Functions lokaal](functions-run-local.md).

>[!IMPORTANT]   
> Azure Functions-runtime 2.0 is in preview en momenteel niet alle functies van Azure Functions worden ondersteund. Zie voor meer informatie, [overzicht van de versies van de Azure Functions runtime](functions-versions.md).

## <a name="automatic-and-manual-version-updates"></a>Automatische en handmatige versie-updates

Functions kunt u richten op een specifieke versie van de runtime met behulp van de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling in een functie-app. De functie-app wordt opgeslagen op de opgegeven primaire versie totdat u zelf kiezen om te verplaatsen naar een nieuwe versie.

Als u alleen de primaire versie (~ 1"voor 1.x) of"bètaversie"voor 2.x opgeeft, wordt de functie-app automatisch bijgewerkt naar de nieuwe secundaire versies van de runtime wanneer deze beschikbaar worden. Nieuwe secundaire versies kunnen niet tot belangrijke wijzigingen. Als u een secundaire versie (bijvoorbeeld ' 1.0.11360') opgeeft, wordt de functie-app op die versie bewaard totdat u deze expliciet wijzigt. 

Wanneer een nieuwe versie openbaar beschikbaar is, geeft u een opdrachtprompt in de portal de kans op omhoog verplaatsen naar deze versie. Nadat u hebt verplaatst naar een nieuwe versie, kunt u altijd gebruiken de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling om weer te verplaatsen naar een eerdere versie.

Een wijziging in de runtimeversie zorgt ervoor dat een functie-app te starten.

De waarden die u kunt instellen in de `FUNCTIONS_EXTENSION_VERSION` app-instelling voor het inschakelen van automatische updates is momenteel '~ 1' voor de runtime 1.x en "bètaversie" voor 2.x.

## <a name="view-the-current-runtime-version"></a>De huidige runtimeversie weergeven

Gebruik de volgende procedure om de runtimeversie die momenteel wordt gebruikt door een functie-app weer te geven. 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de functie-app en klikt u onder **functies geconfigureerd**, kiest u **functie app-instellingen**. 

    ![Instellingen voor functie-app selecteren](./media/functions-versions/add-update-app-setting.png)

2. In de **functie app-instellingen** tabblad, Ga naar de **runtimeversie**. Houd er rekening mee de specifieke runtimeversie en de aangevraagde primaire versie. In het voorbeeld hieronder, de functies\_EXTENSIE\_versie app-instelling is ingesteld op `~1`.
 
   ![Instellingen voor functie-app selecteren](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Doel een versie met behulp van de portal

Wanneer u een versie dan de huidige primaire versie of versie 2.0 moet, stelt de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw functie-app en klikt u onder **functies geconfigureerd**, kiest u **toepassingsinstellingen**.

    ![Instellingen voor functie-app selecteren](./media/functions-versions/add-update-app-setting1a.png)

2. In de **toepassingsinstellingen** tabblad, vinden de `FUNCTIONS_EXTENSION_VERSION` instellen en wijzig de waarde in een geldige versie van de runtime 1.x of `beta` voor versie 2.0. Een tilde met hoofdversie betekent dat de meest recente versie van die primaire versie (bijvoorbeeld, ~ 1') gebruiken. 

    ![De functie-app instellen](./media/functions-versions/add-update-app-setting2.png)

3. Klik op **opslaan** om op te slaan van de update van de instelling van toepassing. 

## <a name="target-a-version-using-azure-cli"></a>Doel een versie met behulp van Azure CLI

 U kunt ook instellen de `FUNCTIONS_EXTENSION_VERSION` vanuit de Azure CLI. Met de Azure-CLI bijwerken de toepassingsinstelling in de functie-app met de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
In deze code, Vervang `<function_app>` met de naam van uw functie-app. Vervang ook `<my_resource_group>` met de naam van de resourcegroep voor uw functie-app. Vervang `<version>` met een geldige versie van de runtime 1.x of `beta` voor versie 2.0. 

U kunt uitvoeren met deze opdracht uit de [Azure Cloud Shell](../cloud-shell/overview.md) door te kiezen **uitproberen** in het vorige codevoorbeeld. U kunt ook de [lokaal Azure CLI](/cli/azure/install-azure-cli) aan deze opdracht wordt uitgevoerd na het uitvoeren van [az login](/cli/azure/reference-index#az-login) aan te melden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doel van de runtime 2.0 in uw lokale ontwikkelomgeving](functions-run-local.md)

> [!div class="nextstepaction"]
> [Zie Opmerkingen bij de Release van runtimeversies](https://github.com/Azure/azure-webjobs-sdk-script/releases)