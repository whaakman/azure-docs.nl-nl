---
title: Een Node.js-toepassing maken in een Azure web-app | Microsoft Docs
description: Implementeer in enkele minuten uw eerste Node.js-app (Hello World) in een App Service-web-app.
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ced6f54603120d8832ee417b02b6673f80a99613
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Een Node.js-toepassing maken in een web-app

Deze zelfstudie om snel aan de slag te gaan, helpt u bij het ontwikkelen van een Node.js-app en het implementeren hiervan in Azure. We voeren de app uit met behulp van een [Azure App Service-plan](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) en maken en configureren hierin een nieuwe web-app met behulp van de Azure CLI. Vervolgens gebruiken we Git om onze Node.js-app te implementeren naar Azure.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

U kunt de onderstaande stappen volgen met behulp van een Mac-, Windows- of Linux-computer. Het uitvoeren van alle onderstaande stappen kost u slechts circa 5 minuten.

## <a name="prerequisites"></a>Vereisten

Voordat u dit voorbeeld maakt, moet u de volgende zaken downloaden en installeren:

* [Git](https://git-scm.com/)
* [Node.js en NPM](https://nodejs.org/)
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Kloon de opslagplaats van de Hello World-voorbeeld-app naar uw lokale computer.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Ga naar de map die de voorbeeldcode bevat.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a>De app lokaal uitvoeren

Voer de toepassing lokaal uit door een terminalvenster te openen en het `npm start`-script voor het voorbeeld te gebruiken om de ingebouwde Node.js http-server te starten.

```bash
npm start
```

Open een webbrowser en navigeer naar het voorbeeld.

```bash
http://localhost:1337
```

Het bericht **Hello World** uit de voorbeeld-app wordt weergegeven op de pagina.

![lokale-host-hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

Druk in uw terminalvenster op **Ctrl + C** om de webserver af te sluiten.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

We gaan nu de Azure CLI 2.0 in een terminalvenster gebruiken om de resources te maken die nodig zijn voor het hosten van onze Node.js-app in Azure. Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```

<!-- ## Configure a Deployment User -->
[!INCLUDE [login-to-azure](../../includes/configure-deployment-user.md)]

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources, zoals web-apps, databases en opslagaccounts, worden geïmplementeerd en beheerd.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service-plan"></a>Een Azure App Service-plan maken

Maak een gratis [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) met de opdracht [az appservice plan create](/cli/azure/appservice/plan#create).

<!--
 An App Service plan represents the collection of physical resources used to ..
-->
[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

In het volgende voorbeeld wordt een App Service-plan gemaakt met de naam `quickStartPlan` en de prijscategorie **Gratis**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Wanneer het App Service-plan is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Een webtoepassing maken

Nu er een App Service-plan is gemaakt, maakt u binnen dit `quickStartPlan` App Service-plan een [web-app](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview). De web-app geeft ons een hostingruimte voor het implementeren van onze code, evenals een URL waarmee we de geïmplementeerde toepassing kunnen bekijken. Gebruik de opdracht [az appservice web create](/cli/azure/appservice/web#create) om de web-app te maken.

Vervang in de onderstaande opdracht de tijdelijke aanduiding `<app_name>` door de naam van uw eigen unieke app. De `<app_name>` wordt gebruikt in de standaard-DNS-site voor de web-app. Als `<app_name>` niet uniek is, wordt het foutbericht "Er bestaan al een website met de naam <naam_app>" weergegeven.

<!-- removed per https://github.com/Microsoft/azure-docs-pr/issues/11878
You can later map any custom DNS entry to the web app before you expose it to your users.
-->

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Blader naar de site om uw nieuwe web-app te bekijken.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-gemaakt](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

We hebben nu een lege nieuwe web-app gemaakt in Azure.

## <a name="configure-local-git-deployment"></a>Lokale Git-implementatie configureren

U kunt uw web-app op verschillende manieren implementeren, zoals met FTP, lokale Git, evenals GitHub, Visual Studio Team Services en Bitbucket.

Gebruik de opdracht [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) voor het configureren van lokale Git-toegang tot de web-app.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Kopieer de uitvoer van de terminal, aangezien deze wordt gebruikt in de volgende stap.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Voeg een externe Azure-instantie toe aan uw lokale Git-opslagplaats.

```bash
git remote add azure <paste-previous-command-output-here>
```

Push naar de externe Azure-instantie om uw app te implementeren. U wordt gevraagd naar het wachtwoord dat u eerder hebt opgegeven bij het maken van de implementatiegebruiker. Zorg ervoor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij de Azure Portal.

```bash
git push azure master
```

Tijdens de implementatie communiceert Azure App Service de voortgang naar Git.

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde toepassing via uw webbrowser.

```bash
http://<app_name>.azurewebsites.net
```

Deze keer wordt de pagina die het bericht Hello World weergeeft, uitgevoerd met onze Node.js-code die wordt uitgevoerd als een Azure App Service-web-app.

## <a name="updating-and-deploying-the-code"></a>De code bijwerken en implementeren

Open met behulp van een lokale teksteditor het bestand `index.js` binnen de Node.js-app en breng een kleine wijziging aan in de tekst in de aanroep naar `response.end`:

```nodejs
response.end("Hello Azure!");
```

Leg uw wijzigingen vast in Git en push de codewijzigingen vervolgens naar Azure.

```bash
git commit -am "updated output"
git push azure master
```

Als de implementatie is voltooid, gaat u terug naar het browservenster dat is geopend in de stap **Bladeren naar de app** en klikt u op Vernieuwen.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Uw nieuwe Azure-web-app beheren

Ga naar Azure Portal om de web-app die u zojuist hebt gemaakt, te bekijken.

Hiervoor moet u zich aanmelden bij [https://portal.azure.com](https://portal.azure.com).

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

U bent aangekomen op de _blade_ van uw web-app (een portalpagina die horizontaal wordt geopend).

Standaard toont de blade van uw web-app de pagina **Overzicht**. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de blade tonen de verschillende configuratiepagina's die u kunt openen.

![App Service-blade in Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Deze tabbladen op de blade bevatten de vele handige functies die kunt u toevoegen aan uw web-app. De volgende lijst bevat slechts enkele van de mogelijkheden:

* Een aangepaste DNS-naam toewijzen
* Een aangepast SSL-certificaat binden
* Continue implementatie inschakelen
* Omhoog en omlaag schalen
* Gebruikersverificatie toevoegen

**Gefeliciteerd!** U hebt uw eerste Node.js-app geïmplementeerd in App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

> [!div class="nextstepaction"]
> [Voorbeelden van CLI-scripts voor web-apps bekijken](app-service-cli-samples.md)

