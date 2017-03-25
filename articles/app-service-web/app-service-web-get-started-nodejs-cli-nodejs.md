---
title: Uw Node.js-web-app in vijf minuten in Azure implementeren | Microsoft Docs
description: Ontdek door implementatie van een Node.js-voorbeeld-app hoe eenvoudig het is om web-apps in App Service uit te voeren. U kunt snel een app gaan ontwikkelen en onmiddellijk de resultaten bekijken.
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 670d38f6-48ad-481c-8d82-bd532501f8db
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 4ddcaabfa116bac5d961bd24c4ed53cf678f0ad7
ms.lasthandoff: 03/21/2017


---
# <a name="deploy-your-first-nodejs-web-app-to-azure-in-five-minutes"></a>Uw eerste Node.js-web-app in vijf minuten in Azure implementeren
[!INCLUDE [app-service-web-selector-get-started-nodejs-cli](../../includes/app-service-web-selector-get-started-nodejs-cli.md)]  

Met deze zelfstudie leert u om uw eerste Node.js-web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Met App Service kunt u web-apps, [back-ends voor mobiele apps](/documentation/learning-paths/appservice-mobileapps/) en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) maken.

U gaat het volgende doen: 

* Een web-app maken in Azure App Service.
* Node.js-voorbeeldcode implementeren.
* Zien hoe de code live in productie wordt uitgevoerd.
* De web-app op dezelfde manier bijwerken als waarop u [Git-doorvoeracties pusht](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](app-service-web-get-started-nodejs-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel
- [Azure CLI 2.0](app-service-web-get-started-nodejs.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="prerequisites"></a>Vereisten
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI](../cli-install-nodejs.md).
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> U kunt [App Service proberen](https://azure.microsoft.com/try/app-service/) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="deploy-a-nodejs-web-app"></a>Een Node.js-web-app implementeren
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
4. Schakel naar een werkmap (`CD`) en ga als volgt te werk om de Node.js-voorbeeld-app te kopiëren:
   
        git clone https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git
5. Schakel naar de opslagplaats van uw voorbeeld-app.
   
        cd app-service-web-nodejs-get-started
6. Maak de App Service-toepassingsbron in Azure met een unieke naam en de implementatiegebruiker die u eerder hebt geconfigureerd. Geef het nummer van de gewenste regio op wanneer dit wordt gevraagd.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![De Azure-resource maken voor uw eerste web-app in Azure](./media/app-service-web-get-started-languages/node-site-create.png)
   
    Uw app is nu gemaakt in Azure. Bovendien is de huidige map voor Git geïnitialiseerd en als Git remote verbonden met de nieuwe App Service-app.
    U kunt naar de URL van de app bladeren (http://&lt;app_naam>.azurewebsites.net), om de mooi vormgegeven standaard-HTML-pagina te bekijken, maar in deze zelfstudie gaan we uw code hier neerzetten.
7. Implementeer de voorbeeldcode in de Azure-app op dezelfde manier als waarop u code zou pushen met Git. Gebruik, wanneer u daarnaar wordt gevraagd, het wachtwoord dat u eerder hebt geconfigureerd.
   
        git push azure master
   
    ![Code pushen naar uw eerste web-app in Azure](./media/app-service-web-get-started-languages/node-git-push.png)
   
    `git push` plaatst niet alleen code in Azure, maar activeert ook implementatietaken in de implementatie-engine. 
    Als er een package.json-bestand aanwezig is in de hoofdmap (opslagplaats) van uw project, worden de vereiste pakketten hersteld door het implementatiescript. 

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
[Maak, configureer en implementeer een Node.js Express-web-app in Azure](app-service-web-nodejs-get-started-cli-nodejs.md). Door deze zelfstudie te volgen, leert u de basisvaardigheden voor het uitvoeren van een Node.js-web-app in Azure, zoals:

* Apps via PowerShell/Bash maken en configureren in Azure.
* De Node.js-versie instellen.
* Een opstartbestand gebruiken dat zich niet in de hoofdmap van de toepassing bevindt.
* Automatiseren met NPM.
* Fout- en uitvoerlogboeken ophalen.

Of doe meer met uw eerste web-app. Bijvoorbeeld:

* Probeer [andere manieren om uw code in Azure te implementeren](web-sites-deploy.md). Als u bijvoorbeeld wilt implementeren vanuit een van uw GitHub-opslagplaatsen, selecteert u in **Implementatieopties** **GitHub** in plaats van **Lokale Git-opslagplaats**.
* Breng uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiewaarschuwingen in. Dit alles met slechts enkele klikken. Zie [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).


