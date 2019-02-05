---
title: Het doel-Azure Functions runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Informatie over het opgeven van de runtimeversie van een functie-app die wordt gehost in Azure.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: glenga
ms.openlocfilehash: 6e8142e391dd02e78be42e1f16ae2626b74c41c3
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734467"
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

Kunt u de runtimeversie die wordt gebruikt door u functie-app. Vanwege het potentieel van belangrijke wijzigingen, moet u de runtimeversie alleen wijzigen voordat u alle functies in uw functie-app hebt gemaakt. Hoewel de runtimeversie wordt bepaald door de `FUNCTIONS_EXTENSION_VERSION` instelt, moet u deze wijziging aanbrengt in Azure portal en niet door de instelling wijzigen rechtstreeks. Dit is omdat de portal voor uw wijzigingen worden gevalideerd en andere gerelateerde wijzigingen aanbrengt, indien nodig.

### <a name="from-the-azure-portal"></a>Vanuit Azure portal

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

### <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Vanuit de Azure CLI

U kunt ook bekijken en stel de `FUNCTIONS_EXTENSION_VERSION` vanuit de Azure CLI.

>[!NOTE]
>Omdat andere instellingen wordt mogelijk beïnvloed door de runtimeversie, moet u de versie in de portal wijzigen. De portal maakt automatisch de andere vereiste updates, zoals Node.js-versie en runtime-stack, wanneer u runtimeversies wijzigt.  

Met de Azure CLI, de huidige runtimeversie met weer de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) opdracht.

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

U kunt bijwerken de `FUNCTIONS_EXTENSION_VERSION` instellen in de functie-app met de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) opdracht.

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
