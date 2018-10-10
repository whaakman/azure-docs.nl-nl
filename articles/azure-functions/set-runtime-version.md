---
title: Het doel-Azure Functions runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Informatie over het opgeven van de runtimeversie van een functie-app die wordt gehost in Azure.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: glenga
ms.openlocfilehash: 6d89746c0a2d4642e5025789d352803195c0a3b9
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886804"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Het doel-Azure Functions runtime-versies

Een functie-app wordt uitgevoerd op een specifieke versie van de Azure Functions-runtime. Er zijn twee primaire versies: [1.x en 2.x](functions-versions.md). Standaard functie-apps die zijn gemaakt, versie 2.x van de runtime. In dit artikel wordt uitgelegd hoe u een functie-app configureren in Azure uit te voeren op de versie die u kiest. Zie voor meer informatie over het configureren van een lokale ontwikkelingsomgeving voor een specifieke versie [Code en test Azure Functions lokaal](functions-run-local.md).

> [!NOTE]
> U kunt de runtimeversie voor een functie-app met een of meer functies niet wijzigen. U moet de Azure-portal gebruiken om het wijzigen van de runtimeversie.

## <a name="automatic-and-manual-version-updates"></a>Automatische en handmatige versie-updates

Functions kunt u richten op een specifieke versie van de runtime met behulp van de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling in een functie-app. De functie-app wordt opgeslagen op de opgegeven primaire versie totdat u zelf kiezen om te verplaatsen naar een nieuwe versie.

Als u alleen de primaire versie ("~ 2" voor 2.x of '~ 1' voor 1.x) opgeeft, wordt de functie-app automatisch bijgewerkt naar de nieuwe secundaire versies van de runtime wanneer deze beschikbaar worden. Nieuwe secundaire versies kunnen niet tot belangrijke wijzigingen. Als u een secundaire versie (bijvoorbeeld ' 2.0.12345') opgeeft, wordt de functie-app is vastgemaakt aan die specifieke versie totdat u deze expliciet wijzigt.

Wanneer een nieuwe versie openbaar beschikbaar is, geeft u een opdrachtprompt in de portal de kans op omhoog verplaatsen naar deze versie. Nadat u hebt verplaatst naar een nieuwe versie, kunt u altijd gebruiken de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling om weer te verplaatsen naar een eerdere versie.

Een wijziging in de runtimeversie zorgt ervoor dat een functie-app te starten.

De waarden die u kunt instellen in de `FUNCTIONS_EXTENSION_VERSION` app-instelling voor het inschakelen van automatische updates zijn momenteel '~ 1' voor de runtime 1.x en "~ 2" voor 2.x.

## <a name="view-and-update-the-current-runtime-version"></a>Weergeven en bijwerken van de huidige runtimeversie

Gebruik de volgende procedure om de runtimeversie die momenteel wordt gebruikt door een functie-app weer te geven.

1. In de [Azure-portal](https://portal.azure.com), blader naar uw functie-app.

1. Onder **functies geconfigureerd**, kiest u **functie app-instellingen**.

    ![Instellingen voor functie-app selecteren](./media/set-runtime-version/add-update-app-setting.png)

1. In de **functie app-instellingen** tabblad, Ga naar de **runtimeversie**. Houd er rekening mee de specifieke runtimeversie en de aangevraagde primaire versie. In het onderstaande voorbeeld de versie is ingesteld op `~2`.

   ![Instellingen voor functie-app selecteren](./media/set-runtime-version/function-app-view-version.png)

1. Als u wilt vastmaken aan de versie 1.x-runtime uw functie-app, kies **~ 1** onder **runtimeversie**. Deze schakeloptie is uitgeschakeld als er functies in uw app.

1. Als u de runtimeversie is gewijzigd, gaat u terug naar de **overzicht** tabblad en kies **opnieuw** de app opnieuw te starten.  De functie-app wordt opnieuw opgestart die worden uitgevoerd op de versie 1.x-runtime en de sjablonen van versie 1.x worden gebruikt wanneer u functies maakt.

## <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Weergeven en bijwerken van de runtimeversie met behulp van Azure CLI

U kunt ook bekijken en stel de `FUNCTIONS_EXTENSION_VERSION` vanuit de Azure CLI.

>[!NOTE]
>Omdat andere instellingen wordt mogelijk beïnvloed door de runtimeversie, moet u de versie in de portal wijzigen. De portal maakt automatisch de andere vereiste updates, zoals Node.js-versie en runtime-stack, wanneer u runtimeversies wijzigt.  

Met de Azure CLI, de huidige runtimeversie met weer de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

In deze code, Vervang `<function_app>` met de naam van uw functie-app. Vervang ook `<my_resource_group>` met de naam van de resourcegroep voor uw functie-app. 

U ziet de `FUNCTIONS_EXTENSION_VERSION` in de volgende uitvoer, die is afgekapt voor de duidelijkheid:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

U kunt bijwerken de `FUNCTIONS_EXTENSION_VERSION` instellen in de functie-app met de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Vervang `<function_app>` met de naam van uw functie-app. Vervang ook `<my_resource_group>` met de naam van de resourcegroep voor uw functie-app. Vervang ook `<version>` met een geldige versie van de runtime 1.x of `~2` voor versie 2.x.

U kunt uitvoeren met deze opdracht uit de [Azure Cloud Shell](../cloud-shell/overview.md) door te kiezen **uitproberen** in het vorige codevoorbeeld. U kunt ook de [lokaal Azure CLI](/cli/azure/install-azure-cli) aan deze opdracht wordt uitgevoerd na het uitvoeren van [az login](/cli/azure/reference-index#az-login) aan te melden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doel van de runtime 2.0 in uw lokale ontwikkelomgeving](functions-run-local.md)

> [!div class="nextstepaction"]
> [Zie Opmerkingen bij de Release van runtimeversies](https://github.com/Azure/azure-webjobs-sdk-script/releases)
