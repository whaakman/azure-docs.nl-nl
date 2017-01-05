---
title: Een web-app maken vanuit Azure Marketplace | Microsoft Docs
description: Lees hoe u vanuit Azure Marketplace een nieuwe WordPress-web-app maakt met behulp van de Azure Portal.
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: 972a296d-f927-470b-8534-0f2cb9eac223
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 00c4336bd5cef4ddc0b92127d0945d39291b9c7f


---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Een web-app maken vanuit Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace maakt een groot aantal populaire web-apps beschikbaar die zijn ontwikkeld door Microsoft, door derden of via OOS-initiatieven (Open Source Software). Bijvoorbeeld WordPress, Umbraco CMS, Drupal, enzovoort. Deze web-apps zijn gebouwd op een breed scala aan populaire frameworks, zoals [PHP] in dit WordPress-voorbeeld, [.NET], [Node.js], [Java] en [Python], om er enkele te noemen. De enige software die u nodig hebt om vanuit Azure Marketplace een web-app te maken, is de browser die u gebruikt voor de [Azure Portal].

In deze zelfstudie leert u het volgende:

* In de Azure App Service een web-app zoeken en maken die op een Azure Marketplace-sjabloon is gebaseerd.
* Azure App Service-instellingen configureren voor de nieuwe web-app.
* Uw web-app starten en beheren.

In deze zelfstudie gaat u een WordPress-blogsite implementeren vanuit Azure Marketplace. Wanneer u de stappen in deze zelfstudie hebt uitgevoerd, hebt u uw eigen WordPress-site in werking in de cloud.

![Voorbeeld van dashboard voor WordPress-web-app][WordPressDashboard1]

Op de WordPress-site die u in deze zelfstudie gaat implementeren, wordt voor de database MySQL gebruikt. Als u in plaats daarvan SQL Database voor de database wilt gebruiken, raadpleeg dan [Project Nami], dat eveneens beschikbaar is via Azure Marketplace.

> [!NOTE]
> U hebt een Microsoft Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u [uw voordelen als Visual Studio-abonnee activeren][activate] of [zich registreren voor een gratis proefversie][free trial].
> 
> Als u met Azure App Service aan de slag wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen]. Daar kunt u direct een tijdelijke en eenvoudige web-app maken in App Service. U hebt hiervoor geen creditcard nodig en u bent nergens toe verplicht.
> 
> 

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Een web-app zoeken en maken in Azure App Service
1. Meld u aan bij de [Azure Portal].
2. Klik op **Nieuw**.
   
    ![Een nieuwe Azure-resource maken][MarketplaceStart]
3. Zoek naar **WordPress** en klik op **WordPress**. (Als u in plaats van MySQL SQL Database wilt gebruiken, zoekt u naar **Project Nami**.)
   
    ![WordPress zoeken in de Marketplace][MarketplaceSearch]
4. Lees de beschrijving van de WordPress-app en klik op **Maken**.
   
    ![Een WordPress-web-app maken][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Azure App Service-instellingen configureren voor uw nieuwe web-app
1. Nadat u een nieuwe web-app hebt gemaakt, wordt de blade Instellingen van WordPress weergegeven. Hiermee voert u de volgende stappen uit:
   
    ![Instellingen voor de WordPress-web-app configureren][ConfigStart]
2. Typ in het vak **Web-app** een naam voor de web-app.
   
    Deze naam moet uniek zijn in het domein azurewebsites.net, omdat de URL van de web-app *{naam}*.azurewebsites.net wordt. Als de ingevoerde naam niet uniek is, ziet u een rood uitroepteken in het tekstvak.
   
    ![De naam van de WordPress-web-app configureren][ConfigAppName]
3. Als u meerdere abonnementen hebt, kiest u het abonnement dat u wilt gebruiken.
   
    ![Het abonnement voor de web-app configureren][ConfigSubscription]
4. Selecteer een **Resourcegroep** of maak een nieuwe.
   
    Zie [Overzicht van Azure Resource Manager][ResourceGroups] voor meer informatie over resourcegroepen.
   
    ![De resourcegroep voor de web-app configureren][ConfigResourceGroup]
5. Selecteer een **App Service-plan/-locatie** of maak een nieuw(e).
   
    Zie [Overzicht van Azure App Service-plannen][AzureAppServicePlans] voor meer informatie over App Service-plannen.
   
    ![Het serviceplan voor de web-app configureren][ConfigServicePlan]
6. Klik op **Database** en geef op de blade **Nieuwe MySQL-database** de vereiste waarden op om uw MySQL-database te configureren.
   
    a. Voer een nieuwe naam in of laat de standaardnaam staan.
   
    b. Laat **Databasetype** ingesteld op **Gedeeld**.
   
    c. Kies de locatie die u ook voor de web-app hebt gekozen.
   
    d. Kies een prijscategorie. **Mercury**: is gratis met minimale verbindingen en schijfruimte. Dit is voldoende voor deze zelfstudie.
   
    e. Accepteer de juridische bepalingen op de blade **Nieuwe MySQL-database** en klik op **OK**.
   
    ![De database-instellingen voor de web-app configureren][ConfigDatabase]
7. Accepteer de juridische bepalingen op de blade **WordPress** en klik op **Maken**.
   
    ![De instellingen voor de web-app voltooien en op OK klikken][ConfigFinished]
   
    De web-app wordt gewoonlijk in nog geen minuut door Azure App Service gemaakt. U kunt de voortgang bekijken door boven aan de portalpagina op het belpictogram te klikken.
   
    ![Voortgangsindicator][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>De WordPress-web-app starten en beheren
1. Wanneer de web-app is gemaakt, navigeert u in de Azure Portal naar de resourcegroep waarin u de toepassing hebt gemaakt. Hier ziet u de web-app en de database.
   
    De extra resource met het pictogram van een gloeilampje is [Application Insights][ApplicationInsights], een resource die bewakingsservices voor uw web-app biedt.
2. Klik op de blade **Resourcegroep** op de regel van de web-app.
   
    ![Uw WordPress-web-app selecteren][WordPressSelect]
3. Klik op de blade Web-apps op **Bladeren**.
   
    ![Naar de WordPress-web-app bladeren][WordPressBrowse]
4. Als u wordt gevraagd om de taal voor de WordPress-blog te selecteren, selecteert u de gewenste taal en klikt u op **Doorgaan**.
   
    ![De taal voor de WordPress-web-app configureren][WordPressLanguage]
5. Voer op de **Welkomstpagina** van WordPress de configuratiegegevens in die WordPress nodig heeft. Klik vervolgens op **WordPress installeren**.
   
    ![De instellingen voor de WordPress-web-app configureren][WordPressConfigure]
6. Meld u aan met de referenties die u op de **Welkomstpagina** hebt gemaakt.  
7. De dashboardpagina van de site wordt geopend met de informatie die u hebt opgegeven.    
   
    ![Het WordPress-dashboard weergeven][WordPressDashboard2]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u gezien hoe u een voorbeeld-web-app kunt maken en implementeren vanuit Azure Marketplace.

Voor meer informatie over werken met App Service-web-apps klikt u op de koppelingen aan de linkerkant van de pagina (in brede browservensters) of boven aan de pagina (in smalle browservensters).

Voor meer informatie over het ontwikkelen van WordPress-web-apps in Azure raadpleegt u [WordPress ontwikkelen met Azure App Service][WordPressOnAzure].

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[App Service uitproberen]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Project Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png



<!--HONumber=Dec16_HO1-->


