---
title: In vijf minuten uw eerste web-app implementeren in Azure | Microsoft Docs
description: Hier ontdekt u door een voorbeeld-app te implementeren hoe eenvoudig het is om web-apps in App Service uit te voeren. U kunt snel een app gaan ontwikkelen en onmiddellijk de resultaten bekijken.
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 65c9bdd9-8763-4c56-8e15-f790992e951e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 2c750583212d7ed13e3bb5f4d0770d52aa9610b2


---
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes"></a>In vijf minuten uw eerste web-app implementeren in Azure
Deze zelfstudie helpt u om uw eerste web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Met App Service kunt u web-apps, [back-ends voor mobiele apps](/documentation/learning-paths/appservice-mobileapps/) en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) maken.

U gaat het volgende doen: 

* Een web-app maken in Azure App Service.
* Voorbeeldcode implementeren (kies tussen ASP.NET, PHP, Node.js, Java of Python).
* Zien hoe de code live in productie wordt uitgevoerd.
* De web-app op dezelfde manier bijwerken als waarop u [Git-doorvoeracties pusht](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](app-service-web-get-started-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel
- [Azure CLI 2.0 (Preview)](app-service-web-get-started.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="prerequisites"></a>Vereisten
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI](../xplat-cli-install.md).
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> U kunt [App Service proberen](https://azure.microsoft.com/try/app-service/) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="deploy-a-web-app"></a>Een web-app implementeren
We gaan een web-app implementeren op Azure App Service.

1. Open een nieuw(e) Windows-opdrachtprompt, PowerShell-venster, Linux-shell of OS X-terminal. Voer `git --version` en `azure --version` uit om te controleren of Git en Azure CLI op uw computer zijn geïnstalleerd.
   
    ![De installatie van CLI-hulpprogramma's testen voor uw eerste web-app in Azure](./media/app-service-web-get-started/1-test-tools.png)
   
    Als u de hulpprogramma's niet hebt geïnstalleerd, raadpleeg dan [Vereisten](#Prerequisites) voor downloadkoppelingen.
2. Meld u als volgt aan bij Azure:
   
        azure login
   
    Volg de aanwijzingen in het Help-bericht om door te gaan met de aanmelding.
   
    ![Aanmelden bij Azure om uw eerste web-app te maken](./media/app-service-web-get-started/3-azure-login.png)
3. Zet Azure CLI in de ASM-modus en stel vervolgens de implementatiegebruiker voor App Service in. U gaat later code implementeren met behulp van de referenties.
   
        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

4. Schakel naar een werkmap (`CD`) en ga als volgt te werk om de voorbeeld-app te kopiëren:
   
        git clone <github_sample_url>
   
    ![Kopieer de voorbeeldcode van de app voor uw eerste web-app in Azure](./media/app-service-web-get-started/2-clone-sample.png)
   
    Voor *&lt;github_sample_url>* gebruikt u een van de volgende URL's, afhankelijk van het framework dat u wilt gebruiken:
   
   * HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
   * ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
   * PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
   * Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
   * Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
   * Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

5. Schakel naar de opslagplaats van uw voorbeeld-app. Bijvoorbeeld:
   
        cd app-service-web-html-get-started

6. Maak de App Service-appresource in Azure met een unieke app-naam en de implementatiegebruiker die u eerder hebt geconfigureerd. Geef het nummer van de gewenste regio op wanneer dit wordt gevraagd.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![De Azure-resource maken voor uw eerste web-app in Azure](./media/app-service-web-get-started/4-create-site.png)
   
    Uw app is nu gemaakt in Azure. Bovendien is de huidige map voor Git geïnitialiseerd en als Git remote verbonden met de nieuwe App Service-app.
    U kunt naar de URL van de app bladeren (http://&lt;app_naam>.azurewebsites.net), om de mooi vormgegeven standaard-HTML-pagina te bekijken, maar in deze zelfstudie gaan we uw code hier neerzetten.

7. Implementeer de voorbeeldcode in de Azure-app op dezelfde manier als waarop u code zou pushen met Git. Gebruik, wanneer u daarnaar wordt gevraagd, het wachtwoord dat u eerder hebt geconfigureerd.
   
        git push azure master
   
    ![Code pushen naar uw eerste web-app in Azure](./media/app-service-web-get-started/5-push-code.png)
   
    Als u een van de taalframeworks hebt gebruikt, ziet u andere uitvoer. `git push` plaatst niet alleen code in Azure, maar activeert ook implementatietaken in de implementatie-engine. Als u package.json- (Node.js) of requirements.txt-bestanden (Python) in de hoofdmap van het project (opslagplaats) hebt, of als er een packages.config-bestand in uw ASP.NET-project staat, worden de vereiste pakketten met de implementatiescripts voor u hersteld. U kunt ook [de Composer-extensie inschakelen](web-sites-php-mysql-deploy-use-git.md#composer) als u composer.json-bestanden in uw PHP-app automatisch wilt verwerken.

Gefeliciteerd, u hebt uw app geïmplementeerd in Azure App Service.

## <a name="see-your-app-running-live"></a>Uw app live in werking zien
Als u uw app in Azure in werking wilt zien, voert u deze opdracht uit vanuit een willekeurige map in de opslagplaats:

    azure site browse

## <a name="make-updates-to-your-app"></a>Updates aanbrengen in uw app
Nu kunt u met Git op elk moment push-acties uitvoeren vanuit het project (opslagplaats) om een actieve site bij te werken. Dit werkt op dezelfde manier als toen u de code voor het eerst implementeerde. Zo hoeft u telkens wanneer u een nieuwe wijziging wilt pushen die u lokaal hebt getest, alleen de volgende opdrachten uit te voeren vanuit de hoofdmap van het project (opslagplaats):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Volgende stappen
Zoek als volgt de meest geschikte ontwikkel- en implementatiestappen voor uw taalframework:

> [!div class="op_single_selector"]
> * [.NET](web-sites-dotnet-get-started.md)
> * [PHP](app-service-web-php-get-started-cli-nodejs.md)
> * [Node.js](app-service-web-nodejs-get-started-cli-nodejs.md)
> * [Python](web-sites-python-ptvs-django-mysql.md)
> * [Java](web-sites-java-get-started.md)
> 
> 

Of doe meer met uw eerste web-app. Bijvoorbeeld:

* Probeer [andere manieren om uw code in Azure te implementeren](web-sites-deploy.md). Als u bijvoorbeeld wilt implementeren vanuit een van uw GitHub-opslagplaatsen, selecteert u in **Implementatieopties** **GitHub** in plaats van **Lokale Git-opslagplaats**.
* Breng uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiewaarschuwingen in. Dit alles met slechts enkele klikken. Zie [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).




<!--HONumber=Feb17_HO2-->


