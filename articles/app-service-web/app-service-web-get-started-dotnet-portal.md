---
title: Binnen vijf minuten een Umbraco-web-app implementeren in Azure Portal | Microsoft Docs
description: Ontdek door implementatie van een ASP.NET-voorbeeld-app hoe eenvoudig het is om web-apps in App Service uit te voeren. U kunt direct de resultaten bekijken.
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/10/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: fa3f31cdd708729071876ffad707bea70567da83
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a>Binnen vijf minuten een Umbraco-web-app implementeren in Azure Portal

Deze zelfstudie helpt u om binnen enkele minuten een [Umbraco](https://our.umbraco.org/)-web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).

![Umbraco-app](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Vereisten
U hebt een Microsoft Azure-account nodig. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> U kunt [App Service proberen](https://azure.microsoft.com/try/app-service/) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="deploy-the-aspnet-app"></a>De ASP.NET-app implementeren
1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Open [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).

    Deze koppeling is een snelkoppeling waarmee u direct een nieuwe Umbraco-app kunt configureren in Azure Portal.

3. In **App-naam** voert u een naam in voor de web-app. Er wordt een groen vinkje weergegeven in het vak als de naam uniek is in het domein `azurewebsites.net`.
   
5. In **Resourcegroep** klikt u op **Nieuw** om een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md) te maken. Geef de groep vervolgens een naam.

7. Klik op **App Service-plan/-locatie** > **Nieuw**. Configureer het [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) als volgt:

    - In **App Service-plan** voert u de gewenste naam in.
    - In **Locatie** kiest u een locatie voor het hosten van uw plan.
    - Klik op **Prijscategorie** en selecteer **F1 Gratis** of een andere categorie die bij u past. Klik dan op **Selecteren**.
    - Klik op **OK**.

    De Umbraco CMS-configuratie ziet er nu uit als in de volgende schermafbeelding:

    ![Configuratie in uitvoering - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Klik op **SQL Database** > **Een nieuwe database maken**. Configureer de SQL-database als volgt:

    - Voer in **Naam** een naam in, zoals **myDB**.
    - Klik op **Prijscategorie** en selecteer **F Gratis** of een andere categorie die bij u past. Klik dan op **Selecteren**.
    - Klik op **Doelserver** > **Een nieuwe server maken**. Configureer de databaseserver als weergegeven:

        - Voer in **Servernaam** een servernaam in. Er wordt een groen vinkje weergegeven in het vak als de naam uniek is in het domein `.database.windows.net`.
        - In **Aanmeldgegevens van serverbeheerder** voert u de gewenste gebruikersnaam in voor de beheerder.
        - In **Wachtwoord** en **Wachtwoord bevestigen** voert u het gewenste wachtwoord in.
        - In Locatie selecteert u de locatie die u wilt gebruiken voor de web-app.
        - Zorg ervoor dat **Toegang van Azure-services tot server toestaan** is geselecteerd.
        - Klik op **Selecteren**.
    
    - Klik op **Selecteren**.

13. Klik op **Web-app-instellingen**, geef een gebruikersnaam en wachtwoord op voor de database en klik op **OK**.

    De Umbraco CMS-configuratie ziet er nu uit als in de volgende schermafbeelding:

    ![Configuratie voltooid - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Klik op **Create**.
    
    Azure maakt nu uw Umbraco-app op basis van uw configuratie. De melding **Implementatie is gestart...** wordt nu weergegeven.

    ![De implementatie is voltooid - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Uw Umbraco-web-app starten en beheren

Wanneer Azure klaar is met het implementeren van de app, krijgt u nog een melding te zien.

![De implementatie is voltooid - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Klik op de melding. Als u de melding hebt gemist, kunt u deze altijd openen door op de bel te klikken (![Melding onderaan - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    U krijgt nu de [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) te zien voor het beheer van uw web-app (*blade*: een portalpagina die horizontaal wordt geopend).

3. Klik boven aan de pagina Overzicht op **Bladeren**.
   
    ![Bladeren - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    U ziet de **welkomstpagina** van Umbraco. Configureer de Umbraco-installatie en ga ermee aan de slag!

    ![Umbraco-configuratie - eerste Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Volgende stappen
* [Een ASP.NET-web-app in Azure App Service implementeren met Visual Studio](web-sites-dotnet-get-started.md) -Informatie over het maken van een nieuwe Azure-web-app via Visual Studio met behulp van één van de meegeleverde toepassingssjablonen.
* [Uw code implementeren in Azure App Service](web-sites-deploy.md) - Informatie over implementeren via FTP of via broncodebeheeropslagplaatsen.
* [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md) - Til uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiewaarschuwingen in. Dit alles met slechts enkele klikken.

