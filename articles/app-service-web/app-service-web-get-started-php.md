---
title: In vijf minuten uw eerste PHP-web-app implementeren in Azure (CLI 2.0 Preview) | Microsoft Docs
description: Ontdek door implementatie van een PHP-voorbeeld-app hoe eenvoudig het is om web-apps in App Service uit te voeren. U kunt snel een app gaan ontwikkelen en onmiddellijk de resultaten bekijken.
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
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: f739bf6101936ff6bb25738e4888476e3b33a38f


---
# <a name="deploy-your-first-php-web-app-to-azure-in-five-minutes-cli-20-preview"></a>In vijf minuten uw eerste PHP-web-app implementeren in Azure (CLI 2.0 Preview)

> [!div class="op_single_selector"]
> * [Eerste HTML-site](app-service-web-get-started-html.md)
> * [Eerste .NET-app](app-service-web-get-started-dotnet.md)
> * [Eerste PHP-app](app-service-web-get-started-php.md)
> * [Eerste Node.js-app](app-service-web-get-started-nodejs.md)
> * [Eerste Python-app](app-service-web-get-started-python.md)
> * [Eerste Java-app](app-service-web-get-started-java.md)
> 
> 

Met deze zelfstudie leert u om uw eerste PHP-web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Met App Service kunt u web-apps, [back-ends voor mobiele apps](/documentation/learning-paths/appservice-mobileapps/) en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) maken.

U gaat het volgende doen: 

* Een web-app maken in Azure App Service.
* PHP-voorbeeldcode implementeren.
* Zien hoe de code live in productie wordt uitgevoerd.
* De web-app op dezelfde manier bijwerken als waarop u [Git-doorvoeracties pusht](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](app-service-web-get-started-php-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel
- [Azure CLI 2.0 (Preview)](app-service-web-get-started-php.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="prerequisites"></a>Vereisten
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI 2.0 Preview](/cli/azure/install-az-cli2).
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> U kunt [App Service proberen](https://azure.microsoft.com/try/app-service/) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="deploy-a-php-web-app"></a>Een PHP-web-app implementeren
1. Open een nieuw(e) Windows-opdrachtprompt, PowerShell-venster, Linux-shell of OS X-terminal. Voer `git --version` en `azure --version` uit om te controleren of Git en Azure CLI op uw computer zijn geïnstalleerd.
   
    ![De installatie van CLI-hulpprogramma's testen voor uw eerste web-app in Azure](./media/app-service-web-get-started-languages/1-test-tools-2.0.png)
   
    Als u de hulpprogramma's niet hebt geïnstalleerd, raadpleeg dan [Vereisten](#Prerequisites) voor downloadkoppelingen.
2. Meld u als volgt aan bij Azure:
   
        az login
   
    Volg de aanwijzingen in het Help-bericht om door te gaan met de aanmelding.
   
    ![Aanmelden bij Azure om uw eerste web-app te maken](./media/app-service-web-get-started-languages/3-azure-login-2.0.png)

3. Stel de implementatiegebruiker in voor App Service. U gaat later code implementeren met behulp van deze referenties.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Maak een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md). Voor deze eerste App Service-zelfstudie hoeft u nog niet precies te weten wat dat inhoudt.

        az group create --location "<location>" --name my-first-app-group

    Gebruik de CLI-opdracht `az appservice list-locations` om te zien welke waarden u kunt gebruiken voor `<location>`.

3. Maak een nieuw 'GRATIS' [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Voor deze eerste App Service-zelfstudie hoeft u alleen te weten dat de web-apps in dit plan niet in rekening worden gebracht.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. Maak een nieuwe web-app met een unieke naam in `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Vervolgens krijgt u de PHP-voorbeeldcode die u gaat implementeren. Schakel naar een werkmap (`CD`) en ga als volgt te werk om de voorbeeld-app te kopiëren:
   
        cd <working_directory>
        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

5. Schakel naar de opslagplaats van uw voorbeeld-app. Bijvoorbeeld:
   
        cd app-service-web-php-get-started
5. Configureer lokale Git-implementatie voor uw App Service-web-app met de volgende opdracht:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    U ziet een JSON-uitvoer zoals de volgende, wat betekent dat de externe Git-opslagplaats is geconfigureerd:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Voeg de URL in de JSON toe als een externe Git voor uw lokale opslagplaats (voor de duidelijkheid `azure` genoemd).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Implementeer de voorbeeldcode in de Azure-app op dezelfde manier als waarop u code zou pushen met Git. Gebruik, wanneer u daarnaar wordt gevraagd, het wachtwoord dat u eerder hebt geconfigureerd.
   
        git push azure master
   
    ![Code pushen naar uw eerste web-app in Azure](./media/app-service-web-get-started-languages/php-git-push.png)
   
    `git push` plaatst niet alleen code in Azure, maar activeert ook implementatietaken in de implementatie-engine. U kunt ook  [de Composer-extensie inschakelen](web-sites-php-mysql-deploy-use-git.md#composer) als u composer.json-bestanden in uw PHP-app automatisch wilt verwerken.

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
[Maak, configureer en implementeer een Laravel-web-app in Azure](app-service-web-php-get-started.md). Door deze zelfstudie te volgen, leert u de basisvaardigheden voor het uitvoeren van een PHP-web-app in Azure, zoals:

* Apps via PowerShell/Bash maken en configureren in Azure.
* De PHP-versie instellen.
* Een opstartbestand gebruiken dat zich niet in de hoofdmap van de toepassing bevindt.
* Composer-automatisering inschakelen.
* Omgevingsspecifieke variabelen openen.
* Veelvoorkomende problemen oplossen.

Of doe meer met uw eerste web-app. Bijvoorbeeld:

* Probeer [andere manieren om uw code in Azure te implementeren](web-sites-deploy.md). Als u bijvoorbeeld wilt implementeren vanuit een van uw GitHub-opslagplaatsen, selecteert u in **Implementatieopties** **GitHub** in plaats van **Lokale Git-opslagplaats**.
* Breng uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiewaarschuwingen in. Dit alles met slechts enkele klikken. Zie [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).




<!--HONumber=Feb17_HO1-->


