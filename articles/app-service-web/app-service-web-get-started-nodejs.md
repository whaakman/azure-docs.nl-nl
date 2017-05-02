---
title: Een Node.js-toepassing maken in een web-app | Microsoft Docs
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
ms.date: 03/28/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: c32cb52e4bb7bacde20e21820f277b4e86877e74
ms.lasthandoff: 04/25/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Een Node.js-toepassing maken in een web-app

Deze zelfstudie om snel aan de slag te gaan, helpt u bij het ontwikkelen van een Node.js-app en het implementeren hiervan in Azure. We voeren de app uit met behulp van een Azure App Service op basis van Linux, en maken en configureren er een nieuwe web-app in met behulp van de Azure CLI. Vervolgens gebruiken we Git om onze Node.js-app te implementeren naar Azure.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

U kunt de onderstaande stappen volgen met behulp van een Mac-, Windows- of Linux-computer. Het uitvoeren van alle onderstaande stappen kost u slechts circa 5 minuten.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u dit voorbeeld uitvoert, moet u het volgende lokaal installeren:

1. [Git downloaden en installeren](https://git-scm.com/)
1. [Node.js en NPM downloaden en installeren](https://nodejs.org/)
1. De [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) downloaden en installeren

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Kloon de opslagplaats van de Hello World-voorbeeld-app naar uw lokale computer.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

> [!TIP]
> U kunt [het voorbeeld ook downloaden](https://github.com/Azure-Samples/nodejs-docs-hello-world/archive/master.zip) als ZIP-bestand en dit uitpakken.

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

## <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

Voor FTP en lokale Git is het noodzakelijk om een implementatiegebruiker te hebben geconfigureerd op de server om uw implementatie te verifiëren. Het maken van een implementatiegebruiker is een eenmalige configuratie. Noteer de gebruikersnaam en het wachtwoord, aangezien deze worden gebruikt in een van de onderstaande stappen.

> [!NOTE]
> Een implementatiegebruiker is vereist voor FTP- en lokale Git-implementatie naar een web-app.
> De `username` en `password` zijn op accountniveau en als zodanig verschillend van de referenties van uw Azure-abonnement. Deze referenties hoeven maar één keer te worden gemaakt.
>

Gebruik de opdracht [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) om uw referenties op accountniveau te maken.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources, zoals web-apps, databases en opslagaccounts, worden geïmplementeerd en beheerd.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service"></a>Een Azure App Service maken

Maak een App Service-plan op basis van Linux met de opdracht [az appservice plan create](/cli/azure/appservice/plan#create).

> [!NOTE]
> Een App Service-plan bestaat uit een verzameling van fysieke resources die worden gebruikt voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een App Service-plan, delen de gedefinieerde resources, zodat u kosten kunt besparen als u meerdere apps host.
>
> In App Service-plannen wordt het volgende gedefinieerd:
> * Regio (Noord-Europa, VS - oost, Zuidoost-Azië)
> * Exemplaargrootte (klein, normaal, groot)
> * Schaal (een, twee of drie exemplaren enz.)
> * SKU (Free, Shared, Basic, Standard, Premium)
>

In het volgende voorbeeld wordt een App Service-plan gemaakt in Linux Workers met de naam `quickStartPlan` en de prijscategorie **Standard**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku S1 --is-linux
```

Wanneer het App Service-plan is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld.

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "kind": "linux",
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

Nu er een App Service-plan is gemaakt, maakt u binnen dit `quickStartPlan` App Service-plan een web-app. De web-app geeft ons een hostingruimte voor het implementeren van onze code, evenals een URL waarmee we de geïmplementeerde toepassing kunnen bekijken. Gebruik de opdracht [az appservice web create](/cli/azure/appservice/web#create) om de web-app te maken.

Vervang in de onderstaande opdracht de tijdelijke aanduiding `<app_name>` door de naam van uw eigen unieke app. De `<app_name>` wordt gebruikt als de standaard-DNS-site voor de web-app. De naam moet uniek zijn in alle apps in Azure. Later kunt u een aangepaste DNS-vermelding toewijzen aan de web-app voordat u deze beschikbaar maakt voor uw gebruikers.

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

We hebben nu een lege nieuwe web-app gemaakt in Azure. We gaan nu onze web-app configureren voor het gebruik van Node.js en onze app hierin implementeren.

## <a name="configure-to-use-nodejs"></a>Configureren voor gebruik van Node.js

Gebruik de opdracht [az appservice web config update](/cli/azure/app-service/web/config#update) voor het configureren van de web-app voor het gebruik van Node.js versie `6.9.3`.

> [!TIP]
> Voor het instellen van de node.js-versie op deze manier wordt een standaardcontainer gebruikt die wordt geleverd door het platform. Als u uw eigen container wilt gebruiken, raadpleegt u de CLI-verwijzing voor de opdracht [az appservice web config container update](/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --linux-fx-version "NODE|6.9.3" --startup-file process.json --name <app_name> --resource-group myResourceGroup
```

## <a name="configure-local-git-deployment"></a>Lokale Git-implementatie configureren

U kunt uw web-app op verschillende manieren implementeren, zoals FTP, lokale Git, evenals GitHub, Visual Studio Team Services en Bitbucket.

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

Push naar de externe Azure-instantie om uw toepassing te implementeren. U wordt gevraagd naar het wachtwoord dat u eerder hebt opgegeven als onderdeel van het maken van de implementatiegebruiker.

```azurecli
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

Als de implementatie is voltooid, gaat u terug naar het browservenster dat is geopend in de stap Bladeren naar de app en klikt u op Vernieuwen.

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

## <a name="next-steps"></a>Volgende stappen

Bekijk vooraf gemaakte [CLI-scripts voor web-apps](app-service-cli-samples.md).

