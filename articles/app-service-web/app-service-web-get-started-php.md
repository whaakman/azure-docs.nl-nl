---
title: Uw eerste PHP-web-app in vijf minuten in Azure maken | Microsoft Docs
description: Ontdek hoe eenvoudig het is om web-apps in App Service uit te voeren door een voorbeeld-app voor PHP te implementeren.
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 9a756618549cafc41c4f09683fd710748bf7b411
ms.lasthandoff: 03/15/2017


---
# <a name="create-your-first-php-web-app-in-azure-in-five-minutes"></a>Uw eerste PHP-web-app in vijf minuten in Azure maken
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Met deze Quickstart leert u in een paar minuten hoe u uw eerste PHP-web-app implementeert in [Azure App Service](../app-service/app-service-value-prop-what-is.md).

Controleer voordat u begint of [de Azure-CLI is geïnstalleerd](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) op uw computer.

## <a name="create-a-php-web-app-in-azure"></a>Een PHP-web-app maken in Azure
   
2. Meld u aan bij Azure door `az login` uit te voeren en de aanwijzingen op het scherm te volgen.
   
    ```azurecli
    az login
    ```
   
3. Maak een [resourcegroep](../azure-resource-manager/resource-group-overview.md). Hier verzamelt u alle Azure-resources die u samen wilt beheren, zoals de web-app en de bijbehorende SQL Database-back-end.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    Gebruik de Azure CLI-opdracht `az appservice list-locations` om te zien welke waarden u kunt gebruiken voor `---location`.

3. Maak een nieuw 'Standard' [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). De laag Standard is vereist voor het uitvoeren van Linux-containers.

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku S1 --is-linux 
    ```

4. Maak een web-app met een unieke naam in `<app_name>`.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Configureer de Linux-container voor het gebruik van de standaard-image van PHP 7.0.6.

    ```azurecli
    az appservice web config update --php-version 7.0.6 --name <app_name> --resource-group myResourceGroup
    ```

4. Implementeer een voorbeeld van een PHP-app vanuit GitHub.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/Azure-Samples/app-service-web-php-get-started.git" --branch master --manual-integration 
    ```

5. Voer deze opdracht uit om uw app live in werking te zien in Azure.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Gefeliciteerd, uw eerste PHP web-app wordt live uitgevoerd in Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk vooraf gemaakte [CLI-scripts voor web-apps](app-service-cli-samples.md).

