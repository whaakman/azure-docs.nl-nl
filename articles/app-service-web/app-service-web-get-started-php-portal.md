---
title: Binnen vijf minuten een WordPress-app implementeren in Azure Portal | Microsoft Docs
description: Hier ontdekt u door een WordPress-app te implementeren hoe eenvoudig het is om web-apps in App Service uit te voeren. U kunt direct de resultaten bekijken.
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/13/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7ef5802866bf96859d3f44cdb58cbb412b08a700
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a>Binnen vijf minuten een WordPress-app implementeren in Azure Portal

In deze zelfstudie ziet u hoe u binnen slechts enkele minuten uw eerste [WordPress](https://wordpress.org/)-web-app implementeert in [Azure App Service](../app-service/app-service-value-prop-what-is.md).

![WordPress-site](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Vereisten
U hebt een Microsoft Azure-account nodig. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> U kunt [App Service proberen](https://azure.microsoft.com/try/app-service/) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="deploy-the-wordpress-app"></a>De WordPress-app implementeren
1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).

    Deze koppeling is een snelkoppeling waarmee u direct een nieuwe WordPress-app kunt configureren in Azure Portal.

3. In **App-naam** voert u een naam in voor de web-app. Er wordt een groen vinkje weergegeven in het vak als de naam uniek is in het domein `azurewebsites.net`.
   
5. In **Resourcegroep** klikt u op **Nieuw** om een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md) te maken. Geef de groep vervolgens een naam.

6. In **Databaseprovider** selecteert u **CleaDB**.

7. Klik op **App Service-plan/-locatie** > **Nieuw**. Configureer het [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) als volgt:

    - In **App Service-plan** voert u de gewenste naam in.
    - In **Locatie** kiest u een locatie voor het hosten van uw plan.
    - Klik op **Prijscategorie** en selecteer **F1 Gratis** of een andere categorie die bij u past. Klik dan op **Selecteren**.
    - Klik op **OK**.

8. Klik op **Database** > **Nieuw**. Configureer de SQL-database als volgt:

    - In **Databasenaam** voert u een naam in voor de database. 
    - In **Locatie** kiest u dezelfde locatie als voor het App Service-plan.
    - Klik op **Prijscategorie** en selecteer **Mercury** of een andere categorie die bij u past. Klik dan op **Selecteren**.
    - Klik op **Juridische voorwaarden** en klik op **Kopen**.
    - Klik op **OK**.

9. Klik op **Create**.

    Azure maakt nu uw WordPress-app op basis van uw configuratie. De melding **Implementatie is gestart...** wordt nu weergegeven.

    ![De implementatie is gestart - eerste WordPress in Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>De WordPress-web-app starten en beheren

Wanneer Azure klaar is met het implementeren van de app, krijgt u nog een melding te zien.

![De implementatie is voltooid - eerste WordPress in Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Klik op de melding. Als u de melding hebt gemist, kunt u deze altijd openen door op de bel te klikken (![Melding onderaan - eerste WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    U krijgt nu de [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) te zien voor het beheer van uw web-app (*blade*: een portalpagina die horizontaal wordt geopend).

3. Klik boven aan de pagina Overzicht op **Bladeren**.
   
    ![Bladeren - eerste WordPress in Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    U ziet de **welkomstpagina** van WordPress. Configureer de WordPress-installatie en ga ermee aan de slag!

    ![Configuratie van WordPress - eerste WordPress in Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Volgende stappen
* [Een Laravel-web-app maken, configureren en implementeren in Azure](app-service-web-php-get-started.md) - Informatie over de algemene vaardigheden die nodig zijn voor het uitvoeren van PHP-web-apps in Azure, zoals:

    * Apps via PowerShell/Bash maken en configureren in Azure.
    * De PHP-versie instellen.
    * Een opstartbestand gebruiken dat zich niet in de hoofdmap van de toepassing bevindt.
    * Composer-automatisering inschakelen.
    * Omgevingsspecifieke variabelen openen.
    * Veelvoorkomende problemen oplossen.

* [Uw code implementeren in Azure App Service](web-sites-deploy.md) - Informatie over implementeren via FTP of via broncodebeheeropslagplaatsen.
* [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md) - Til uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiewaarschuwingen in. Dit alles met slechts enkele klikken.

