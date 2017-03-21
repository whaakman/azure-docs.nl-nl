---
title: Uw eerste Java-web-app in vijf minuten in Azure maken | Microsoft Docs
description: Hier ontdekt u door een voorbeeld-app te implementeren hoe eenvoudig het is om web-apps in App Service uit te voeren.
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: e48e03e86a325b8f39809a49cdd19820dfa78bdc
ms.lasthandoff: 03/10/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Uw eerste Java-web-app in vijf minuten in Azure maken
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Met deze Quickstart leert u in een paar minuten hoe u uw eerste Java-web-app implementeert in [Azure App Service](../app-service/app-service-value-prop-what-is.md).

Controleer voordat u begint of [de Azure-CLI is geïnstalleerd](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) op uw computer.

## <a name="create-a-java-web-app-in-azure"></a>Een Java-web-app maken in Azure App Service
2. Meld u aan bij Azure door `az login` uit te voeren en de aanwijzingen op het scherm te volgen.
   
    ```azurecli
    az login
    ```
   
3. Maak een [resourcegroep](../azure-resource-manager/resource-group-overview.md). Hier verzamelt u alle Azure-resources die u samen wilt beheren, zoals de web-app en de bijbehorende SQL Database-back-end.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    Gebruik de Azure CLI-opdracht `az appservice list-locations` om te zien welke waarden u kunt gebruiken voor `---location`.

3. Maak een 'GRATIS' [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
    ```

4. Maak een nieuwe web-app met een unieke naam in `<app_name>`.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Implementeer een voorbeeld van een Java-app vanuit GitHub.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/azure-appservice-samples/JavaCoffeeShopTemplate.git" --branch master --manual-integration 
    ```


5. Voer deze opdracht uit om uw app live in werking te zien in Azure.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Gefeliciteerd, uw eerste Java-web-app wordt live uitgevoerd in Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk vooraf gemaakte [CLI-scripts voor web-apps](app-service-cli-samples.md).

