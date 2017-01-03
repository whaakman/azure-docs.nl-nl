---
title: Aan de slag met Node.js-web-apps in Azure App Service | Microsoft Docs
description: Leer hoe u een Node.js-toepassing implementeert in een web-app in Azure App Service.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: wpickett
editor: 
ms.assetid: fb2b90c8-02b6-4700-929b-5de9a35d67cc
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: f595be46983bf07783b529de885d889c18fdb61a
ms.openlocfilehash: 9667d805fee3277275a71e6907d0abffb35a3c48


---
# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Aan de slag met Node.js-web-apps in Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In deze zelfstudie ziet u hoe u vanaf een opdrachtregelomgeving, zoals cmd.exe of bash, een eenvoudige [Node.js]-toepassing maakt en implementeert in [Azure App Service]. De instructies in deze zelfstudie kunnen worden uitgevoerd in elk besturingssysteem waarmee Node.js kan worden uitgevoerd.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

<a name="prereq"></a>

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](app-service-web-nodejs-get-started-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel
- [Azure CLI 2.0 (Preview)](app-service-web-nodejs-get-started.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="prerequisites"></a>Vereisten
* [Node.js]
* [Bower]
* [Yeoman]
* [Git]
* [Azure CLI 2.0 Preview](/cli/azure/install-az-cli2)
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie] of [uw voordelen als Visual Studio-abonnee activeren].

> [!NOTE]
> U kunt [App Service proberen](http://go.microsoft.com/fwlink/?LinkId=523751) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="create-and-configure-a-simple-nodejs-app-for-azure"></a>Een eenvoudige Node.js-app voor Azure maken en configureren
1. Open de gewenste opdrachtregelterminal en installeer de [Express-generator voor Yeoman].
   
        npm install -g generator-express

2. `CD` in een werkmap en genereer een express-app met de volgende syntaxis:
   
        yo express
   
    Kies de volgende opties wanneer dit wordt gevraagd:  
   
    `? Would you like to create a new directory for your project?` **Ja**  
    `? Enter directory name` **{app-naam}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **Geen**  
    `? Select a database to use:` **Geen**  
    `? Select a build tool to use:` **Grunt**

3. `CD` in de hoofdmap van de nieuwe app en start de app om te controleren of deze in uw ontwikkelingsomgeving wordt uitgevoerd:
   
        npm start
   
    Navigeer in uw browser naar <http://localhost:3000> om te controleren of de startpagina van Express wordt geopend. Wanneer u hebt geverifieerd dat de app correct wordt uitgevoerd, gebruikt u `Ctrl-C` om deze te stoppen.

6. Open het bestand ./config/config.js in de hoofdmap van de toepassing en wijzig de productiepoort in `process.env.port`. De eigenschap `production` in het object `config` moet eruitzien als in het volgende voorbeeld:
   
        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }
   
    > [!NOTE] 
    > Azure App Service voert standaard Node.js-toepassingen uit met de `production`-omgevingsvariabelen (`process.env.NODE_ENV="production"`.
    > Daarom zorgt uw configuratie hier ervoor dat uw Node.js-app kan reageren op webaanvragen bij de standaardpoort waarnaar iisnode luistert.
    >
    >

7. Open./package.json en voeg de `engines`-eigenschap toe om [de gewenste versie van Node.js op te geven](#version).
   
        "engines": {
            "node": "6.9.1"
        }, 

8. Sla uw wijzigingen op, initialiseer vervolgens een Git-opslagplaats in de hoofdmap van uw toepassing en voer uw code door:
   
        git add .
        git add -f config
        git commit -m "{your commit message}"

## <a name="deploy-your-nodejs-app-to-azure"></a>Uw Node.js-app implementeren in Azure

1. Aanmelden bij Azure ([Azure CLI 2.0 Preview](#prereq) is vereist):
   
        az login
   
    Volg de aanwijzing om de aanmelding in een browser voort te zetten met het Microsoft-account waarmee u uw Azure-abonnement hebt afgesloten.

3. Stel de implementatiegebruiker in voor App Service. U gaat later code implementeren met behulp van deze referenties.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Maak een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md). Voor deze eerste PHP-zelfstudie hoeft u nog niet precies te weten wat dat inhoudt.

        az group create --location "<location>" --name my-nodejs-app-group

    Gebruik de CLI-opdracht `az appservice list-locations` om te zien welke waarden u kunt gebruiken voor `<location>`.

3. Maak een nieuw 'GRATIS' [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Voor deze eerste PHP-zelfstudie hoeft u alleen te weten dat de web-apps in dit plan niet in rekening worden gebracht.

        az appservice plan create --name my-nodejs-appservice-plan --resource-group my-nodejs-app-group --sku FREE

4. Maak een nieuwe web-app met een unieke naam in `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-nodejs-app-group --plan my-nodejs-appservice-plan

5. Configureer een lokale Git-implementatie voor uw nieuwe web-app. Doe dit met de volgende opdracht:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-nodejs-app-group

    U ziet JSON-uitvoer zoals hieronder, wat betekent dat de externe Git-opslagplaats is geconfigureerd:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Voeg de URL in de JSON toe als een externe Git voor uw lokale opslagplaats (voor de duidelijkheid `azure` genoemd).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Implementeer de voorbeeldcode in de externe Git `azure`. Wordt hierom gevraagd, gebruik dan de implementatiereferenties die u eerder hebt geconfigureerd.

        git push azure master
   
    De Express-generator bevat al een bestand .gitignore. `git push` gebruikt dus geen bandbreedte voor het uploaden van de map node_modules/.

9. Ten slotte gaat u als volgt te werk om de live Azure-app in de browser te starten:
   
        az appservice web browse --name <app_name> --resource-group my-nodejs-app-group
   
    U ziet nu hoe uw node.js-web-app live in Azure App Service wordt uitgevoerd.
   
    ![Voorbeeld van het bladeren naar de geïmplementeerde toepassing.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>De Node.js-web-app bijwerken
Als u updates wilt aanbrengen in een Node.js-web-app die wordt uitgevoerd in App Service, voert u `git add`, `git commit` en `git push` uit, net zoals toen u de web-app de eerste keer implementeerde.

## <a name="how-app-service-deploys-your-nodejs-app"></a>Hoe uw Node.js-web-app wordt geïmplementeerd door App Service
Azure App Service gebruikt [iisnode] om Node.js-apps uit te voeren. De engines Azure CLI 2.0 Preview en Kudu (Git-implementatie) werken samen voor een gestroomlijnde ervaring bij het ontwikkelen en implementeren van Node.js-apps vanaf de opdrachtregel. 

* U kunt een iisnode.yml-bestand maken in de hoofdmap en dit gebruiken om de iisnode-eigenschappen aan te passen. Alle configureerbare instellingen zijn [hier](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/iisnode.yml) gedocumenteerd.
* Op `git push azure master` worden de volgende implementatietaken door Kudu geautomatiseerd:
  
  * Voer `npm install --production` uit als package.json in de hoofdmap van de opslagplaats staat.
  * Genereer een Web.config voor iisnode die naar uw startscript in package.json wijst (bijvoorbeeld server.js of app.js).
  * Pas Web.config aan om de app gereed te maken voor foutopsporing met Node-Inspector.

## <a name="use-a-nodejs-framework"></a>Een Node.js-framework gebruiken
Als u voor de ontwikkeling van apps een populair Node.js-framework gebruikt, zoals [Sails.js][SAILSJS] of [MEAN.js][MEANJS], kunt u de apps implementeren in App Service. Maar populaire Node.js-frameworks hebben zo hun eigenaardigheden. Bovendien worden hun pakketafhankelijkheden voortdurend bijgewerkt. Via App Service hebt u echter inzage in de logboeken stdout en stderr, zodat u precies weet wat er met uw app gebeurt en dienovereenkomstig wijzigingen kunt aanbrengen. Zie [Logboeken stdout en stderr ophalen uit iisnode](#iisnodelog) voor meer informatie.

In de volgende zelfstudies leert u hoe u in App Service met een specifiek framework werkt:

* [Een Sails.js-web-app implementeren in Azure App Service]
* [In Azure App Service een Node.js-chattoepassing maken met Socket.IO]
* [io.js gebruiken met Azure App Service Web Apps]

<a name="version"></a>

## <a name="use-a-specific-nodejs-engine"></a>Een specifieke Node.js-engine gebruiken
In een reguliere werkstroom instrueert u App Service om een bepaalde Node.js-engine te gebruiken, net zoals in package.json.
Bijvoorbeeld:

    "engines": {
        "node": "6.9.1"
    }, 

De implementatie-engine Kudu bepaalt aan de hand van de volgende volgorde welke Node.js-engine er wordt gebruikt:

* Kijk eerst in iisnode.yml of `nodeProcessCommandLine` is opgegeven. Zo ja, maak hier dan gebruik van.
* Bekijk vervolgens package.json om na te gaan of `"node": "..."` is opgegeven in het `engines`-object. Zo ja, maak hier dan gebruik van.
* Kies een standaardversie van Node.js.

Voor de bijgewerkte lijst met alle ondersteunde versies van Node.js/NPM in Azure App Service opent u de volgende URL voor uw app:

    https://<app_name>.scm.azurewebsites.net/api/diagnostics/runtime

> [!NOTE]
> Het is raadzaam dat u de gewenste Node.js-engine expliciet definieert. De standaardversie van Node.js kan worden gewijzigd, en mogelijk worden er foutmeldingen in uw Azure-web-app weergegeven omdat de standaardversie van Node.js niet geschikt is voor uw app.
> 
> 

<a name="iisnodelog"></a>

## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>De logboeken stdout en stderr ophalen uit iisnode
Volg deze stappen om iisnode-logboeken te lezen.

> [!NOTE]
> Wanneer u deze stappen hebt voltooid, is het mogelijk dat de logboekbestanden pas worden gegenereerd wanneer er een fout optreedt.
> 
> 

1. Open het bestand iisnode.yml dat door Azure CLI 2.0 Preview wordt verstrekt.
2. Stel de volgende twee parameters in: 
   
        loggingEnabled: true
        logDirectory: iisnode
   
    Met deze twee parameters wordt aan iisnode in App Service doorgegeven dat de uitvoer van stdout en stderror moet worden geplaatst in de map D:\home\site\wwwroot\**iisnode**.
3. Sla de wijzigingen op en push ze met de volgende Git-opdrachten naar Azure:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    Iisnode is nu geconfigureerd. In de volgende stappen ziet u hoe u deze logboeken opent.
4. Ga in uw browser naar de Kudu-console voor foutopsporing. Deze bevindt zich op:
   
        https://{appname}.scm.azurewebsites.net/DebugConsole 
   
    Dit is een andere URL dan die van de web-app, omdat '*.scm.*' is toegevoegd aan de DNS-naam. Als u deze aanvulling op de URL weglaat, krijgt u een 404-fout.
5. Navigeer naar D:\home\site\wwwroot\iisnode
   
    ![Navigeren naar de locatie van de iisnode-logboekbestanden][iislog-kudu-console-find]
6. Klik op het pictogram **Bewerken** voor het logboek dat u wilt lezen. U kunt desgewenst ook op **Downloaden** of **Verwijderen** klikken.
   
    ![Een iisnode-logboekbestand openen][iislog-kudu-console-open]
   
    Nu kunt u het logboek bekijken om u te helpen bij het opsporen van fouten in uw App Service-implementatie.
   
    ![Een iisnode-logboekbestand bestuderen][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Fouten in een app opsporen met Node-Inspector
Als u Node-Inspector gebruikt om fouten in een Node.js-app op te sporen, kunt u dit hulpprogramma gebruiken voor live App Service-apps. Node-Inspector wordt vooraf geïnstalleerd, samen met de installatie van iisnode voor App Service. Als u de implementatie uitvoert via Git, bevat de automatisch gegenereerde Web.config van Kudu bovendien al de configuratie die u nodig hebt om Node-Inspector in te schakelen.

Volg deze stappen om Node-Inspector in te schakelen:

1. Open iisnode.yml in de hoofdmap van uw opslagplaats en geef de volgende parameters op: 
   
        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll
2. Sla de wijzigingen op en push ze met de volgende Git-opdrachten naar Azure:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
3. Navigeer nu naar het startbestand van uw app (dat is opgegeven in het startscript in uw package.json) met /debug toegevoegd aan de URL. Bijvoorbeeld:
   
        http://{appname}.azurewebsites.net/server.js/debug
   
    Of
   
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Meer bronnen
* [Een Node.js-versie opgeven in een Azure-toepassing](../nodejs-specify-node-version-azure-apps.md)
* [Aanbevolen procedures en gids voor probleemoplossing voor Node.js-toepassingen in Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
* [Fouten opsporen in een Node.js web-app in Azure App Service](web-sites-nodejs-debug.md)
* [Node.js-modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Web-apps van Azure App Service: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
* [Node.js Developer Center](/develop/nodejs/)
* [Aan de slag met web-apps in Azure App Service](app-service-web-get-started.md)
* [De geheimen van de Kudu-console voor foutopsporing]

<!-- URL List -->

[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[uw voordelen als Visual Studio-abonnee activeren]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[In Azure App Service een Node.js-chattoepassing maken met Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Een Sails.js-web-app implementeren in Azure App Service]: ./app-service-web-nodejs-sails.md
[De geheimen van de Kudu-console voor foutopsporing]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express-generator voor Yeoman]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[io.js gebruiken met Azure App Service Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[aanmelden voor een gratis proefversie]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png



<!--HONumber=Jan17_HO1-->


