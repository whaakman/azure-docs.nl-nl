---
title: Het doel van Azure Functions-runtime-versies | Microsoft Docs
description: Functies ondersteunt meerdere versies van de runtime. Informatie over het opgeven van de runtimeversie van een Azure gehoste functie-app.
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: glenga
ms.openlocfilehash: 26d4276a0a550d78a9c7657c464bd3320c956fb0
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Het doel van Azure Functions-runtime-versies

De Azure Functions-runtime implementeert uw code zonder server wordt uitgevoerd in Azure. U vindt deze runtime in verschillende omgevingen, dan wordt gehost in Azure. De [kernonderdelen van Azure Functions](functions-run-local.md) deze runtime implementeert op uw ontwikkelcomputer. [Azure Functions-Runtime](functions-runtime-overview.md) kunt u functies gebruiken in on-premises omgevingen. 

Functies ondersteunt meerdere primaire versies van de runtime. Een update hoofdversie kan grote wijzigingen veroorzaken. Dit onderwerp wordt beschreven hoe u uw apps functie naar een specifieke runtimeversie wanneer deze wordt gehost in Azure kunt richten. 

Functies kunt u het doel van een specifieke primaire versie van de runtime met behulp van de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling in functie-app. Dit geldt voor zowel openbare en preview-versies. De functie-app wordt opgeslagen op de opgegeven primaire runtimeversie totdat u expliciet wilt verplaatsen naar een nieuwe versie. U functie-app wordt bijgewerkt naar nieuwe secundaire versies van de runtime wanneer deze beschikbaar komen. Secundaire versie updates doen geen grote wijzigingen.  

Wanneer een nieuwe primaire versie openbaar beschikbaar is, krijgt u de kans te verplaatsen naar die versie wanneer u uw app in de functie in de portal weergeeft. Nadat u hebt verplaatst naar een nieuwe versie, kunt u altijd gebruiken de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling om terug te gaan naar een eerdere runtimeversie.

Elke wijziging van de runtimeversie zorgt ervoor dat de functie-app te starten. Release-opmerkingen voor alle runtime-versies (zowel primaire als secundaire) worden gepubliceerd in de [GitHub-opslagplaats](https://github.com/Azure/azure-webjobs-sdk-script/releases).   
## <a name="view-the-current-runtime-version"></a>De huidige runtimeversie weergeven

Gebruik de volgende procedure om de specifieke runtime-versie die momenteel wordt gebruikt door uw app in de functie weer te geven. 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de functie-app en klikt u onder **functies geconfigureerd**, kies **werken app-instellingen**. 

    ![Selecteer de functie app-instellingen](./media/functions-versions/add-update-app-setting.png)

2. In de **werken app-instellingen** tabblad, zoek de **runtimeversie**. Noteer de specifieke runtimeversie en de aangevraagde primaire versie. In het volgende voorbeeld wordt de primaire versie is ingesteld op `~1.0`.
 
   ![Selecteer de functie app-instellingen](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-functions-version-20-runtime"></a>Doel van de runtime van Functions versie 2.0

>[!IMPORTANT]   
> Azure Functions-runtime 2.0 is een Preview-versie en op dit moment niet alle functies van Azure Functions worden ondersteund. Zie voor meer informatie [Azure Functions-runtime 2.0 bekende problemen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues)  

<!-- Add a table comparing the 1.x and 2.x runtime features-->

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

U kunt de functie-app verplaatsen naar de runtime versie 2.0 preview in de Azure portal. In de **werken app-instellingen** Kies **beta** onder **runtimeversie**.  

   ![Selecteer de functie app-instellingen](./media/functions-versions/function-app-view-version.png)

Deze instelling komt overeen met de instelling voor de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling naar `beta`. Kies de **~ 1** knop om terug te gaan naar het huidige openbaar primaire versie ondersteund. U kunt ook de Azure CLI gebruiken om bij te werken met deze toepassingsinstelling. 

## <a name="target-a-specific-runtime-version-from-the-portal"></a>Gericht op een specifieke runtimeversie van de portal

Wanneer u moet een primaire versie dan de huidige primaire doel of versie 2.0, moet u instellen de `FUNCTIONS_EXTENSION_VERSION` toepassingsinstelling.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de functie-app en klikt u onder **functies geconfigureerd**, kies **toepassingsinstellingen**.

    ![Selecteer de functie app-instellingen](./media/functions-versions/add-update-app-setting1a.png)

2. In de **toepassingsinstellingen** tabblad, zoek de `FUNCTIONS_EXTENSION_VERSION` instellen en wijzig de waarde in een geldige primaire versie van de runtime 1.x of `beta` voor versie 2.0. 

    ![De functie app-instelling instellen](./media/functions-versions/add-update-app-setting2.png)

3. Klik op **opslaan** om op te slaan van de instelling toepassingsupdate. 

## <a name="target-a-specific-version-using-azure-cli"></a>Gericht op een specifieke versie met Azure CLI

 U kunt ook instellen de `FUNCTIONS_EXTENSION_VERSION` met Azure CLI. De toepassingsinstelling in de functie-app met behulp van de Azure CLI bijwerken de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Vervang in deze code `<function_app>` met de naam van de functie-app. Vervang ook `<my_resource_group>` met de naam van de resourcegroep voor de functie-app. Vervang `<version>` met een geldige primaire versie van de runtime 1.x of `beta` voor versie 2.0. 

U kunt uitvoeren met deze opdracht uit de [Azure Cloud Shell](../cloud-shell/overview.md) door te kiezen **Try it** in het vorige codevoorbeeld. U kunt ook de [Azure CLI lokaal](/cli/azure/install-azure-cli) deze opdracht wordt uitgevoerd na het uitvoeren van [az aanmelding](/cli/azure#az_login) aan te melden.
