---
title: Een statische HTML-web-app in vijf minuten maken in Azure | Microsoft Docs
description: Hier ontdekt u door een voorbeeld-app te implementeren hoe eenvoudig het is om web-apps in App Service uit te voeren.
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/08/2017
ms.author: riande
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 895906e1ab4bc50093ed3b18f043c3dd515ca054
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Een statische HTML-web-app in vijf minuten maken in Azure

Deze snelstartgids laat zien hoe u een eenvoudige HTML+CSS-site naar Azure implementeert. U gaat de app uitvoeren met behulp van een [Azure App Service-plan](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) en maakt een web-app met behulp van de [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli). U gebruikt Git om de app te implementeren naar Azure. Het duurt ongeveer vijf minuten om de zelfstudie te voltooien, nadat de vereiste onderdelen zijn geïnstalleerd.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="prerequisites"></a>Vereisten

Voordat u dit voorbeeld maakt, moet u de volgende onderdelen downloaden en installeren:

- [Git](https://git-scm.com/)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Kloon de opslagplaats van de voorbeeld-app naar uw lokale computer in een terminalvenster:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="view-the-html"></a>De HTML weergeven

Ga naar de map die de voorbeeld-HTML bevat. Open het bestand *index.html* in uw browser.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [login-to-azure](../../includes/login-to-azure.md)] 
[!INCLUDE [configure-deployment-user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [app-service-web-quickstart1](../../includes/app-service-web-quickstart1.md)] 

Een [web-app](app-service-web-overview.md) maken in het App Service-plan `quickStartPlan`. De web-app biedt hostruimte voor uw code en biedt een URL waarmee u de geïmplementeerde app kunt weergeven.

[!INCLUDE [app-service-web-quickstart2](../../includes/app-service-web-quickstart2.md)] 

De pagina wordt uitgevoerd als een web-app in Azure App Service:

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="update-and-redeploy-the-app"></a>De app bijwerken en opnieuw implementeren

Open het bestand *index.html*. Wijzig de opmaak. Wijzig bijvoorbeeld `Hello world!` in `Hello Azure!`

Leg uw wijzigingen vast in Git en push de codewijzigingen vervolgens naar Azure.

```bash
git commit -am "updated HTML"
git push azure master
```

Als de implementatie is voltooid, vernieuwt u uw browser om de wijzigingen te bekijken.

[!INCLUDE [manage-azure-web-app](../../includes/manage-azure-web-app.md)]


[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden van [CLI-scripts voor web-apps](app-service-cli-samples.md).
- Leer hoe u [Een aangepaste domeinnaam toewijst](app-service-web-tutorial-custom-domain.md), zoals contoso.com, aan een [App Service-app](app-service-web-tutorial-custom-domain.md).
