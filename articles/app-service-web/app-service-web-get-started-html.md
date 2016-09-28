<properties 
    pageTitle="Uw eerste web-app in vijf minuten in Azure implementeren | Microsoft Azure" 
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
    
# In vijf minuten uw eerste web-app implementeren in Azure

Met deze zelfstudie leert u om een eenvoudige HTML-CSS-web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Met App Service kunt u web-apps, [back-ends voor mobiele apps](/documentation/learning-paths/appservice-mobileapps/) en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) maken.

U gaat het volgende doen: 

- Een web-app maken in Azure App Service.
- Er HTML en CSS voor implementeren.
- Zien hoe de pagina’s live in productie wordt uitgevoerd.
- De inhoud op dezelfde manier bijwerken als waarop u [Git-doorvoeracties pusht](https://git-scm.com/docs/git-push).

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

## Inhoud in de web-app implementeren

Nu gaat u met Git inhoud in Azure implementeren.

5. Schuif in de blade van de web-app omlaag naar **Implementatieopties** of zoek deze optie en klik erop. 

    ![](./media/app-service-web-get-started-languages/deploy-web-app-deployment-options.png)

6. Klik op **Bron kiezen** > **Lokale Git-opslagplaats** > **OK**.

7. In de blade van de web-app klikt u op **Referenties voor implementatie**.

8. Stel uw implementatiereferenties in en klik op **Opslaan**.

7. Schuif in de blade van de web-app omlaag naar **Eigenschappen** of zoek deze optie en klik erop. Klik naast de **Git-URL** op de knop **Kopiëren**.

    ![](./media/app-service-web-get-started-languages/deploy-web-app-properties.png)

    Nu kunt u uw inhoud met Git gaan implementeren.

1. Schakel in de opdrachtregelterminal naar een werkmap (`CD`) en ga als volgt te werk om de voorbeeld-app te kopiëren:

        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

    ![Kopieer de voorbeeldcode van de app voor uw eerste web-app in Azure](./media/app-service-web-get-started-languages/html-git-clone.png)

2. Schakel naar de opslagplaats van uw voorbeeld-app. Bijvoorbeeld: 

        cd app-service-web-html-get-started

3. Stel de Git remote voor uw Azure-app in op de Git-URL die u enkele stappen eerder hebt gekopieerd vanuit de portal.

        git remote add azure <giturlfromportal>

4. Implementeer de voorbeeldcode in de Azure-app op dezelfde manier als waarop u code zou pushen met Git:

        git push azure master

    ![Code pushen naar uw eerste web-app in Azure](./media/app-service-web-get-started-languages/html-git-push.png)    

Dat is alles. De code wordt nu live uitgevoerd in Azure. Navigeer in uw browser naar http://*&lt;appname>*.azurewebsites.net om de code in actie te zien. 

## Updates aanbrengen in uw app

Nu kunt u met Git op elk moment pushacties uitvoeren vanuit het project (opslagplaats) om een actieve site bij te werken. Dit werkt op dezelfde manier als waarop u de inhoud voor het eerst implementeerde. Zo hoeft u telkens wanneer u een nieuwe wijziging wilt pushen die u lokaal hebt getest, alleen de volgende opdrachten uit te voeren vanuit de hoofdmap van het project (opslagplaats):

    git add .
    git commit -m "<your_message>"
    git push azure master

## Volgende stappen

Zoek als volgt de meest geschikte ontwikkel- en implementatiestappen voor uw taalframework:

> [AZURE.SELECTOR]
- [.NET](web-sites-dotnet-get-started.md)
- [PHP](app-service-web-php-get-started.md)
- [Node.js](app-service-web-nodejs-get-started.md)
- [Python](web-sites-python-ptvs-django-mysql.md)
- [Java](web-sites-java-get-started.md)

Of doe meer met uw eerste web-app. Bijvoorbeeld:

- Probeer [andere manieren om uw code in Azure te implementeren](../app-service-web/web-sites-deploy.md). Als u bijvoorbeeld wilt implementeren vanuit een van uw GitHub-opslagplaatsen, selecteert u in **Implementatieopties** **GitHub** in plaats van **Lokale Git-opslagplaats**.
- Til uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiesignalen in. Dit alles met slechts enkele klikken. Zie [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).




<!---HONumber=Sep16_HO4-->


