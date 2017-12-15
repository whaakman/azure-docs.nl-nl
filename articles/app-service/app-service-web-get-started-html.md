---
title: Een statische HTML-web-app maken in Azure | Microsoft Docs
description: Ontdek hoe eenvoudig het is om web-apps uit te voeren in App Service door een voorbeeld van een statische HTML-app te implementeren.
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/26/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ee3b53f9e68a739a39478045debbb45dff9d7ed9
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a>Een statische HTML-web-app maken in Azure

[Azure Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie.  Deze quickstart laat zien hoe u een eenvoudige HTML+CSS-site naar Azure Web Apps implementeert. U maakt de web-app via de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), en u gebruikt Git om HTML-voorbeeldinhoud in de web-app te implementeren.

![Startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

U kunt de onderstaande stappen volgen met behulp van een Mac-, Windows- of Linux-computer. Vanaf het moment dat de vereiste onderdelen zijn geïnstalleerd, duurt het ongeveer vijf minuten om de stappen uit te voeren.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze quickstart:

- <a href="https://git-scm.com/" target="_blank">Git installeren</a>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer in een terminalvenster de volgende opdracht uit om de opslagplaats van de voorbeeld-app te klonen op uw lokale computer.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

Ga naar de map die de voorbeeldcode bevat.

```bash
cd html-docs-hello-world
```

## <a name="view-the-html"></a>De HTML weergeven

Ga naar de map die de voorbeeld-HTML bevat. Open het bestand *index.html* in uw browser.

![Startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Lege pagina van web-app](media/app-service-web-get-started-html/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Bladeren naar de app

Ga in een browser naar de URL van de Azure-web-app: `http://<app_name>.azurewebsites.net`.

De pagina wordt als een web-app uitgevoerd in Azure App Service.

![Startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Gefeliciteerd!** U hebt uw eerste HTML-app geïmplementeerd in App Service.

## <a name="update-and-redeploy-the-app"></a>De app bijwerken en opnieuw implementeren

Open het bestand de *index.html* in een teksteditor en wijzig wat code. Wijzig bijvoorbeeld de tekst van de H1-kop 'Azure App Service - Sample Static HTML Site' in 'Azure App Service'.

Leg in het lokale terminalvenster uw wijzigingen vast in Git en push de codewijzigingen vervolgens naar Azure.

```bash
git commit -am "updated HTML"
git push azure master
```

Als de implementatie is voltooid, vernieuwt u uw browser om de wijzigingen te bekijken.

![Bijgewerkte startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Uw nieuwe Azure-web-app beheren

Ga naar <a href="https://portal.azure.com" target="_blank">Azure Portal</a> om de web-app te beheren die u hebt gemaakt.

Klik in het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-get-started-html/portal1.png)

De pagina Overzicht van uw web-app wordt weergegeven. Hier kunt u algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. 

![App Service-blade in Azure Portal](./media/app-service-web-get-started-html/portal2.png)

Het linkermenu bevat een aantal pagina's voor het configureren van uw app. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aangepast domein toewijzen](app-service-web-tutorial-custom-domain.md)
