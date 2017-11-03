---
title: Doorlopende implementatie in Azure App Service | Microsoft Docs
description: Meer informatie over hoe u continue implementatie voor Azure App Service kunt inschakelen.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: ef5924607868bcb3dc35e279539b78d5a0e17c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continue implementatie voor Azure App Service
Deze zelfstudie leert u hoe u een continue implementatiewerkstroom configureren voor uw [Azure Web Apps](app-service-web-overview.md). App Service-integratie met BitBucket, GitHub en [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) schakelt een continue implementatie-werkstroom waarbij Azure in de meest recente updates van uw project ophaalt op een van deze services worden gepubliceerd. Continue implementatie is een goede optie voor projecten waar meerdere en regelmatige bijdragen worden geïntegreerd.

Voor informatie over het configureren van continue implementatie handmatig van een cloud-opslagplaats niet vermeld in de Azure-Portal (zoals [GitLab](https://gitlab.com/)), Zie [instellen van continue implementatie met handmatige stappen](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Continue implementatie inschakelen
Continue implementatie inschakelen:

1. Publiceer de inhoud van uw app in de opslagplaats die wordt gebruikt voor de continue implementatie.  
    Zie [Een repo maken (GitHub)], [Een repo maken (BitBucket)] en [Aan de slag met VSTS] voor meer informatie over het publiceren van uw project bij deze services.
2. Klik in de menu-blade van uw app in [Azure Portal] op **APP-IMPLEMENTATIE > Implementatieopties**. Klik op **Bron kiezen** en selecteer vervolgens de implementatiebron.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > Raadpleeg deze [zelfstudie](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App) als u een VSTS-account voor App Service-implementatie wilt configureren.
   > 
   > 
3. Voltooi de autorisatiewerkstroom.
4. Kies in de blade **Implementatiebron** welk project en welke branche u als bron wilt gebruiken voor de implementatie. Wanneer u klaar bent, klikt u op **OK**.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > Wanneer u continue implementatie met GitHub of BitBucket inschakelt, worden openbare en particuliere projecten weergegeven.
   > 
   > 
   
    In App Service wordt een koppeling met de geselecteerde opslagplaats gemaakt. Vervolgens worden de bestanden van de opgegeven branche opgehaald en wordt een kloon van de opslagplaats voor uw App Service-app bewaard. Wanneer u continue implementatie in VSTS configureert vanuit Azure Portal, wordt de integratie uitgevoerd met de [Kudu-implementatie-engine](https://github.com/projectkudu/kudu/wiki) van App Service. Deze engine automatiseert al build- en implementatietaken voor elk geval van `git push`. U hoeft continue implementaties in VSTS niet afzonderlijk in te stellen. Wanneer dit proces is voltooid, wordt in de blade **Implementatieopties** van de app een actieve implementatie weergegeven om aan te geven dat de implementatie is voltooid.
5. U kunt controleren of de app is geïmplementeerd door te klikken op de **URL** boven aan de blade van de app in Azure Portal.
6. Als u wilt verifiëren dat de continue implementatie wordt uitgevoerd vanuit de opslagplaats die u hebt gekozen, forceert u een wijziging in de opslagplaats. De wijzigingen moeten in de app worden weergegeven vlak nadat de geforceerde wijzigingen in de opslagplaats zijn doorgevoerd. U kunt controleren of de wijzigingen zijn doorgevoerd door de update te bekijken in de blade **Implementatieopties** van uw app.

## <a name="VSsolution"></a>Continue implementatie van een Visual Studio-oplossing
Het pushen van een Visual Studio-oplossing naar Azure App Service is net zo gemakkelijk als het pushen van een eenvoudig index.html-bestand. Het App Service-implementatieproces stroomlijnt alle informatie, inclusief het herstellen van NuGet-afhankelijkheden en het ontwikkelen van de binaire waarden van toepassingen. U kunt u beperken tot het volgen van de aanbevolen broncodebeheerprocedures voor het onderhouden van code in de Git-opslagplaats en de rest overlaten aan de App Service-implementatie.

De stappen voor het pushen van uw Visual Studio-oplossing naar App Service zijn dezelfde als in de [vorige sectie](#overview), mits u de oplossing en de opslagplaats als volgt configureert:

* Gebruik de optie voor Visual Studio-broncodebeheer om een `.gitignore`-bestand, zoals hieronder weergegeven, te genereren of voeg handmatig een `.gitignore`-toe bestand aan de hoofdmap van uw opslagplaats met inhoud die vergelijkbaar is met dit [.gitignore-voorbeeld](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Voeg de mapstructuur van de hele oplossing toe aan uw opslagplaats en plaats het .sln-bestand in de hoofdmap van de opslagplaats.

Wanneer u de opslagplaats hebt ingesteld zoals hier is beschreven, en u uw app in Azure hebt geconfigureerd voor continue publicatie vanuit een van de online Git-opslagplaatsen, kunt u uw ASP.NET-toepassing lokaal ontwikkelen in Visual Studio uw code continu implementeren door simpelweg uw wijzigingen naar uw online Git-opslagplaats te pushen.

## <a name="disableCD"></a>Continue implementatie uitschakelen
Continue implementatie uitschakelen:

1. Klik in de menu-blade van uw app in [Azure Portal] op **APP-IMPLEMENTATIE > Implementatieopties**. Klik vervolgens op **Verbinding verbreken** in de blade **Implementatieopties**.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Antwoord **Ja** op het bevestigingsbericht en keer vervolgens terug naar de blade van uw app. Klik op **APP-IMPLEMENTATIE > Implementatieopties** als u publicaties vanaf een andere bron wilt instellen.

## <a name="additional-resources"></a>Aanvullende resources
* [Veelvoorkomende problemen met continue implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [PowerShell voor Azure gebruiken]
* [Azure-opdrachtregelprogramma's voor Mac en Linux gebruiken]
* [Git-documentatie]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Gebruik Azure voor het automatisch genereren van een pijplijn CI/CD voor het implementeren van een app ASP.NET 4](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

[Azure Portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[PowerShell voor Azure gebruiken]: /powershell/azureps-cmdlets-docs
[Azure-opdrachtregelprogramma's voor Mac en Linux gebruiken]:../cli-install-nodejs.md
[Git-documentatie]: http://git-scm.com/documentation

[Een repo maken (GitHub)]: https://help.github.com/articles/create-a-repo
[Een repo maken (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Aan de slag met VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
