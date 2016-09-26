<properties 
    pageTitle="Uw eerste PHP-web-app in vijf minuten in Azure implementeren | Microsoft Azure" 
    description="Ontdek hoe eenvoudig het is om web-apps in App Service uit te voeren door een voorbeeld-app te implementeren. Ontwikkel snel uw eigen app en bekijk onmiddellijk de resultaten." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016" 
    ms.author="cephalin"
/>
    
# Uw eerste PHP-web-app in vijf minuten in Azure implementeren

Met deze zelfstudie leert u om uw eerste PHP-web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Met App Service kunt u web-apps, [back-ends voor mobiele apps](/documentation/learning-paths/appservice-mobileapps/) en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) maken.

U gaat het volgende doen: 

- Een web-app maken in Azure App Service.
- PHP-voorbeeldcode implementeren.
- Zien hoe de code live in productie wordt uitgevoerd.
- De web-app op dezelfde manier bijwerken als waarop u [Git-doorvoeracties pusht](https://git-scm.com/docs/git-push).

## Vereisten

- [Installeer Git](http://www.git-scm.com/downloads). Controleer of de installatie is geslaagd door `git --version` uit te voeren vanuit een nieuwe Windows-opdrachtprompt, een PowerShell-venster, Linux-shell of OS X-terminal.
- Verkrijg een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] U kunt [App Service proberen](http://go.microsoft.com/fwlink/?LinkId=523751) zonder een Azure-account. U kunt een beginners-app maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.

<a name="create"></a>
## Een web-app maken

1. Meld u met uw Azure-account aan bij de [Azure-portal](https://portal.azure.com).

2. Klik in het menu aan de linkerkant op **Nieuw** > **Web en mobiel** > **Web-app**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Gebruik in de blade voor het maken van de app de volgende instellingen voor de nieuwe app:

    - **App-naam**: voer een unieke naam in.
    - **Resourcegroep**: selecteer **Nieuwe maken** en geef de resourcegroep een naam.
    - **App Service-plan/-locatie**: klik hier om te configureren en klik vervolgens op **Nieuwe maken** om de naam, locatie en prijscategorie van het App Service-plan in te stellen. Gebruik gerust de prijscategorie **Gratis**.

    Wanneer u klaar bent, ziet de blade voor het maken van de app er als volgt uit:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Klik onderaan op **Maken**. U kunt bovenaan op het **melding**spictogram klikken als u de voortgang wilt bekijken.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Wanneer de implementatie is voltooid, ziet u deze melding. Klik op het bericht als u de blade van uw implementatie wilt openen.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Klik in de blade **Implementatie is voltooid** op de koppeling **Resource** om de blade van de nieuwe web-app te openen.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## Code voor de web-app implementeren

Nu gaat u met Git een stukje code in Azure implementeren.

5. Schuif in de blade van de web-app omlaag naar **Implementatieopties** of zoek deze optie en klik erop. 

    ![](./media/app-service-web-get-started-languages/deploy-web-app-deployment-options.png)

6. Klik op **Bron kiezen** > **Lokale Git-opslagplaats** > **OK**.

7. In de blade van de web-app klikt u op **Referenties voor implementatie**.

8. Stel uw implementatiereferenties in en klik op **Opslaan**.

7. Schuif in de blade van de web-app omlaag naar **Eigenschappen** of zoek deze optie en klik erop. Klik naast de **Git-URL** op de knop **Kopiëren**.

    ![](./media/app-service-web-get-started-languages/deploy-web-app-properties.png)

    Nu kunt u uw code met Git gaan implementeren.

1. Schakel in de opdrachtregelterminal naar een werkmap (`CD`) en ga als volgt te werk om de voorbeeld-app te kopiëren:

        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

    ![Kopieer de voorbeeldcode van de app voor uw eerste web-app in Azure](./media/app-service-web-get-started-languages/php-git-clone.png)

    Voor *&lt;github_sample_url>* gebruikt u een van de volgende URL's, afhankelijk van het framework dat u wilt gebruiken:

2. Schakel naar de opslagplaats van uw voorbeeld-app. Bijvoorbeeld: 

        cd app-service-web-html-get-started

3. Stel de Git remote voor uw Azure-app in op de Git-URL die u enkele stappen eerder hebt gekopieerd vanuit de portal.

        git remote add azure <giturlfromportal>

4. Implementeer de voorbeeldcode in de Azure-app op dezelfde manier als waarop u code zou pushen met Git:

        git push azure master

    ![Code pushen naar uw eerste web-app in Azure](./media/app-service-web-get-started-languages/php-git-push.png)    

    Als u een van de taalframeworks hebt gebruikt, ziet u andere uitvoer. Dat komt omdat `git push` niet alleen code in Azure plaatst, maar ook implementatietaken in de implementatie-engine activeert. Als er een package.json-bestand aanwezig is in de hoofdmap (opslagplaats) van uw project, worden de vereiste pakketten hersteld door het implementatiescript. U kunt ook [de Composer-extensie inschakelen](web-sites-php-mysql-deploy-use-git.md#composer) als u composer.json-bestanden in uw PHP-app automatisch wilt verwerken.

Dat is alles. De code wordt nu live uitgevoerd in Azure. Navigeer in uw browser naar http://*&lt;appname>*.azurewebsites.net om de code in actie te zien. 

## Updates aanbrengen in uw app

Nu kunt u met Git op elk moment pushacties uitvoeren vanuit het project (opslagplaats) om een actieve site bij te werken. Dit werkt op dezelfde manier als waarop u de code voor het eerst implementeerde. Zo hoeft u telkens wanneer u een nieuwe wijziging wilt pushen die u lokaal hebt getest, alleen de volgende opdrachten uit te voeren vanuit de hoofdmap van het project (opslagplaats):

    git add .
    git commit -m "<your_message>"
    git push azure master

## Volgende stappen

[Maak, configureer en implementeer een Laravel-web-app in Azure](app-service-web-php-get-started.md). Door deze zelfstudie te volgen, leert u de basisvaardigheden voor het uitvoeren van een PHP-web-app in Azure, zoals:

- Apps via PowerShell/Bash maken en configureren in Azure.
- De PHP-versie instellen.
- Een opstartbestand gebruiken dat zich niet in de hoofdmap van de toepassing bevindt.
- Componistautomatisering inschakelen.
- Omgevingsspecifieke variabelen openen.
- Veelvoorkomende problemen oplossen.

Of doe meer met uw eerste web-app. Bijvoorbeeld:

- Probeer [andere manieren om uw code in Azure te implementeren](../app-service-web/web-sites-deploy.md). Als u bijvoorbeeld wilt implementeren vanuit een van uw GitHub-opslagplaatsen, selecteert u in **Implementatieopties** **GitHub** in plaats van **Lokale Git-opslagplaats**.
- Til uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiesignalen in. Dit alles met slechts enkele klikken. Zie [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).




<!--HONumber=Sep16_HO4-->


