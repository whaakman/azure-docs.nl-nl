---
title: Een Node.js-web-app maken in Azure App Service | Microsoft Docs
description: Leer hoe u een Node.js-toepassing implementeert in een web-app in Azure App Service.
services: app-service\web
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 85af23df-54af-4430-8d77-a1f97e2f5b10
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 2537b0c4dcc71d02d25959a0b21d96bce7deaac6


---
# <a name="create-a-nodejs-web-app-in-azure-app-service"></a>Een Node.js-web-app maken in Azure App Service
> [!div class="op_single_selector"]
> * [.Net](web-sites-dotnet-get-started.md)
> * [Node.js](web-sites-nodejs-develop-deploy-mac.md)
> * [Java](web-sites-java-get-started.md)
> * [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
> * [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
> * [Python](web-sites-python-ptvs-django-mysql.md)
> 
> 

In deze zelfstudie ziet u hoe u met behulp van [Git](http://git-scm.com) een eenvoudige [Node.js](http://nodejs.org)-toepassing maakt en implementeert in een [web-app](app-service-web-overview.md) in [Azure App Service](../app-service/app-service-value-prop-what-is.md). De instructies in deze zelfstudie kunnen worden uitgevoerd in elk besturingssysteem waarmee Node.js kan worden uitgevoerd.

U leert het volgende:

* Hoe u met behulp van Azure Portal een web-app maakt in Azure App Service.
* Hoe u een Node.js-toepassing in de web-app implementeert door naar de Git-opslagplaats van de web-app te pushen.

Met de voltooide toepassing wordt de korte tekenreeks 'Hello World' in de browser weergegeven.

![Een browser waarin het bericht 'Hello World' wordt weergegeven][helloworld-completed]

In het [Node.js Developer Center](/develop/nodejs/) vindt u zelfstudies en voorbeeldcode met complexere Node.js-toepassingen. Ook kunt u hier terecht voor andere onderwerpen over het gebruik van Node.js in Azure.

> [!NOTE]
> U hebt een Microsoft Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u [uw voordelen als Visual Studio-abonnee activeren](/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) of [zich aanmelden voor een gratis proefversie](/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Als u met Azure App Service aan de slag wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](http://go.microsoft.com/fwlink/?LinkId=523751). Daar kunt u direct een eenvoudige, tijdelijke web-app maken in App Service. U hebt hiervoor geen creditcard nodig en bent nergens toe verplicht.
> 
> 

## <a name="create-a-web-app-and-enable-git-publishing"></a>Een web-app maken en Git-publicatie inschakelen
Volg deze stappen als u een web-app in Azure App Service wilt maken en Git-publicatie wilt inschakelen. 

[Git](http://git-scm.com/) is een gedistribueerd versiebeheersysteem waarmee u een Azure-website kunt implementeren. U slaat de code die u voor uw web-app schrijft, op in een lokale Git-opslagplaats en u implementeert uw code in Azure door deze naar een externe opslagplaats te pushen. Deze implementatiemethode is een functie van Web Apps van App Service.  

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik in de linkerbovenhoek van Azure Portal op het pictogram **+ Nieuw**.
3. Klik op **Web en mobiel** en vervolgens op **Web-app**.
   
    ![][portal-quick-create]
4. Typ in het vak **Web-app** een naam voor de web-app.
   
    Deze naam moet uniek zijn in het domein azurewebsites.net, omdat de URL van de web-app {naam}.azurewebsites.net wordt. Als de ingevoerde naam niet uniek is, ziet u een rood uitroepteken in het tekstvak.
5. Selecteer een **Abonnement**.
6. Selecteer een **Resourcegroep** of maak een nieuwe.
   
    Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.
7. Selecteer een **App Service-plan/-locatie** of maak een nieuw(e).
   
    Zie [Overzicht van Azure App Service-plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) voor meer informatie over App Service-plannen
8. Klik op **Create**.
   
    ![][portal-quick-create2]
   
    In korte tijd, meestal binnen een minuut, is de nieuwe web-app in Azure gemaakt.
9. Klik op **Web-apps > {uw nieuwe web-app}**.
   
    ![](./media/web-sites-nodejs-develop-deploy-mac/gotowebapp.png)
10. Klik op de blade **Web-app** op het onderdeel **Implementatie**.
    
    ![][deployment-part]
11. Klik op de blade **Continue implementatie** op **Bron kiezen**.
12. Klik op **Lokale Git-opslagplaats** en vervolgens op **OK**.
    
    ![][setup-git-publishing]
13. Stel implementatiereferenties in als u dat nog niet hebt gedaan.
    
    a. Klik op de blade Web-app op **Instellingen > implementatiereferenties**.
    
    ![][deployment-credentials]
    
    b. Maak een gebruikersnaam en wachtwoord. 
    
    ![](./media/web-sites-nodejs-develop-deploy-mac/setdeploycreds.png)
14. Klik op de blade Web-app op **Instellingen** en vervolgens op **Eigenschappen**.
    
    Voor publicatie pusht u naar een externe Git-opslagplaats. De URL voor de opslagplaats wordt vermeld onder **GIT-URL**. U hebt deze URL verderop in de zelfstudie nodig.
    
    ![][git-url]

## <a name="build-and-test-your-application-locally"></a>De toepassing lokaal bouwen en testen
In deze sectie maakt u een bestand **server.js**, dat een enigszins gewijzigde versie bevat van het 'Hello World'-voorbeeld uit [nodejs.org]. Met de code wordt process.env.PORT toegevoegd als de poort waarnaar moet worden geluisterd bij uitvoering in een Azure-web-app.

1. Maak een map met de naam *helloworld*.
2. Gebruik een teksteditor om in de map *helloworld* een nieuw bestand te maken met de naam **server.js**.
3. Kopieer de volgende code naar het bestand **server.js** en sla het bestand op:
   
        var http = require('http')
        var port = process.env.PORT || 1337;
        http.createServer(function(req, res) {
          res.writeHead(200, { 'Content-Type': 'text/plain' });
          res.end('Hello World\n');
        }).listen(port);
4. Open de opdrachtregel en gebruik de volgende opdracht om de web-app lokaal te starten.
   
        node server.js
5. Open uw webbrowser en navigeer naar http://localhost:1337. 
   
    Er wordt nu een webpagina met 'Hello World' weergegeven, zoals u in de volgende schermafbeelding ziet.
   
    ![Een browser waarin het bericht 'Hello World' wordt weergegeven][helloworld-localhost]

## <a name="publish-your-application"></a>Uw toepassing publiceren
1. Installeer Git als u dat nog niet hebt gedaan.
   
    Op de [downloadpagina van Git](http://git-scm.com/download) vindt u installatie-instructies voor uw platform.
2. Schakel vanaf de opdrachtregel naar de map **helloworld** en voer de volgende opdracht uit om een lokale Git-opslagplaats te initialiseren.
   
        git init
3. Gebruik de volgende opdrachten om bestanden toe te voegen aan de bibliotheek:
   
        git add .
        git commit -m "initial commit"
4. Voeg een Git remote toe om updates te pushen naar de web-app die u eerder hebt gemaakt. Gebruik hiervoor de volgende opdracht:
   
        git remote add azure [URL for remote repository]
5. Gebruik de volgende opdracht om uw wijzigingen naar Azure te pushen:
   
        git push azure master
   
    U wordt gevraagd naar het wachtwoord dat u eerder hebt gemaakt. De uitvoer lijkt op die in het volgende voorbeeld.
   
        Counting objects: 3, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (3/3), 374 bytes, done.
        Total 3 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '5ebbe250c9'.
        remote: Preparing files for deployment.
        remote: Deploying Web.config to enable Node.js activation.
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
         * [new branch]      master -> master
6. Als u uw app wilt bekijken, klikt u op de knop **Bladeren** in het gedeelte **Web-app** van Azure Portal.
   
    ![Knop Bladeren](./media/web-sites-nodejs-develop-deploy-mac/browsebutton.png)
   
    ![Hello World in Azure](./media/web-sites-nodejs-develop-deploy-mac/helloworldazure.png)

## <a name="publish-changes-to-your-application"></a>Wijzigingen in uw toepassing publiceren
1. Open het bestand **server.js** in een teksteditor en wijzig 'Hello World\n' in 'Hello Azure\n'. 
2. Sla het bestand op.
3. Schakel vanaf de opdrachtregel naar de map **helloworld** en voer de volgende opdrachten uit:
   
        git add .
        git commit -m "changing to hello azure"
        git push azure master
   
    U wordt nogmaals om uw wachtwoord gevraagd.
4. Vernieuw het browservenster waarin u naar de URL van de web-app bent gegaan.
   
    ![Een webpagina waarop 'Hello Azure' wordt weergeven][helloworld-completed]

## <a name="roll-back-a-deployment"></a>Een implementatie terugdraaien
Op de blade **Web-app** kunt u op **Instellingen > Continue implementatie** klikken als u de implementatiegeschiedenis op de blade **Implementaties** wilt bekijken. Als u wilt terugkeren naar een eerdere implementatie, selecteert u deze en klikt u op de blade **Implementatiedetails** op **Opnieuw implementeren**.

## <a name="next-steps"></a>Volgende stappen
U hebt een Node.js-toepassing geïmplementeerd in een web-app in Azure App Service. Voor meer informatie over hoe Node.js-toepassingen worden uitgevoerd in web-apps van App Service, raadpleegt u [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx) en [Een Node.js-versie opgeven in een Azure-toepassing](../nodejs-specify-node-version-azure-apps.md).

Node.js biedt een uitgebreid ecosysteem van modules die kunnen worden gebruikt door uw toepassingen. Als u wilt weten hoe Web Apps met modules werkt, raadpleegt u [Node.js-modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md).

Als u problemen met uw toepassing ondervindt nadat deze is geïmplementeerd in Azure, raadpleegt u [Fouten opsporen in een Node.js-toepassing in Azure App Service](web-sites-nodejs-debug.md) voor meer informatie over het diagnosticeren van het probleem.

In dit artikel wordt Azure Portal gebruikt om een web-app te maken. Met de [Azure-opdrachtregelinterface](../xplat-cli-install.md) of [Azure PowerShell](/powershell/azureps-cmdlets-docs) kunt u dezelfde bewerkingen uitvoeren.

In het [Node.js Developer Center](/develop/nodejs/) vindt u meer informatie over het ontwikkelen van Node.js-toepassingen in Azure.

[helloworld-completed]: ./media/web-sites-nodejs-develop-deploy-mac/helloazure.png
[helloworld-localhost]: ./media/web-sites-nodejs-develop-deploy-mac/helloworldlocal.png
[portal-quick-create]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website.png
[portal-quick-create2]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website2.png
[setup-git-publishing]: ./media/web-sites-nodejs-develop-deploy-mac/setup_git_publishing.png
[go-to-dashboard]: ./media/web-sites-nodejs-develop-deploy-mac/go_to_dashboard.png
[deployment-part]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-part.png
[deployment-credentials]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-credentials.png
[git-url]: ./media/web-sites-nodejs-develop-deploy-mac/git-url.png



<!--HONumber=Jan17_HO1-->


