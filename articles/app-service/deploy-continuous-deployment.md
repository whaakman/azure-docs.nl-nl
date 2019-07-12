---
title: Continue implementatie - Azure App Service | Microsoft Docs
description: Meer informatie over hoe u continue implementatie voor Azure App Service kunt inschakelen.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: cephalin;dariagrigoriu
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 3a207f5513c24f2c33571df929f7490ec7f2eba5
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836858"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continue implementatie in Azure App Service

[Azure App Service](overview.md) wordt continue implementatie vanuit GitHub, BitBucket, ingeschakeld en [Azure opslagplaatsen](https://azure.microsoft.com/services/devops/repos/) opslagplaatsen door te halen in de meest recente updates. Dit artikel ziet u hoe u de Azure portal gebruiken voor uw app via de Kudu-build-service continu te implementeren of [Azure pijplijnen](https://azure.microsoft.com/services/devops/pipelines/). 

Zie voor meer informatie over de services voor bron [een repo maken (GitHub)], [een repo maken (BitBucket)], of [Maak een nieuwe Git-opslagplaats (Azure-opslagplaatsen)].

Het handmatig configureren van continue implementatie vanuit de opslagplaats van een cloud die de portal niet rechtstreeks worden ondersteund, zoals [GitLab](https://gitlab.com/), Zie [instellen van continue implementatie met behulp van handmatige stappen](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Azure App Service autoriseren 

Gebruik Azure-opslagplaatsen, zorg ervoor dat uw organisatie Azure DevOps is gekoppeld aan uw Azure-abonnement. Zie voor meer informatie, [een DevOps-Services van Azure-account instellen zodat deze op een web-app implementeren kunt](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Toestaan dat Azure App Service verbinden met uw opslagplaats voor Bitbucket of GitHub. U hoeft alleen te autoriseren met een bron control-service één keer. 

1. Selecteer **App Services** in de [Azure-portal](https://portal.azure.com) linkernavigatiebalk en selecteer vervolgens de web-app die u wilt implementeren. 
   
1. Selecteer op de pagina app- **Implementatiecentrum** in het menu links.
   
1. Op de **Implementatiecentrum** weergeeft, schakelt **GitHub** of **Bitbucket**, en selecteer vervolgens **autoriseren**. 
   
   ![Selecteer source-control-service, vervolgens autoriseren.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Aanmelden bij de service, indien nodig, en volg de aanwijzingen voor autorisatie. 

## <a name="enable-continuous-deployment"></a>Continue implementatie inschakelen 

Nadat u een source-control-service wilt machtigen, uw app configureren voor continue implementatie via de ingebouwde [Kudu App Service-buildserver](#option-1-use-the-app-service-build-service), of via [Azure pijplijnen](#option-2-use-azure-pipelines). 

### <a name="option-1-use-the-app-service-build-service"></a>Optie 1: De App Service-build-service gebruiken

U kunt de ingebouwde Kudu App Service-buildserver continu implementeren vanuit GitHub, Bitbucket of Azure-opslagplaatsen. 

1. Selecteer **App Services** in de [Azure-portal](https://portal.azure.com) linkernavigatiebalk en selecteer vervolgens de web-app die u wilt implementeren. 
   
1. Selecteer op de pagina app- **Implementatiecentrum** in het menu links.
   
1. Selecteer uw geautoriseerde broncodebeheerprovider op de **Implementatiecentrum** pagina en selecteer **doorgaan**. Voor GitHub of Bitbucket, u kunt ook selecteren **account wijzigen** het geautoriseerde account wijzigen. 
   
   > [!NOTE]
   > Gebruik Azure-opslagplaatsen, zorg ervoor dat uw organisatie Azure DevOps-Services is gekoppeld aan uw Azure-abonnement. Zie voor meer informatie, [een DevOps-Services van Azure-account instellen zodat deze op een web-app implementeren kunt](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   
1. Voor GitHub of Azure-opslagplaatsen, op de **Build-provider** weergeeft, schakelt **App Service-service bouwen**, en selecteer vervolgens **doorgaan**. Bitbucket maakt altijd gebruik van de App-build-service.
   
   ![App Service buildservice selecteert en vervolgens doorgaan.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. Op de **configureren** pagina:
   
   - Vervolgkeuzelijst voor GitHub, en selecteer de **organisatie**, **opslagplaats**, en **vertakking** u wilt implementeren, voortdurend.
     
     > [!NOTE]
     > Als u alle opslagplaatsen niet ziet, moet u mogelijk om te autoriseren Azure App Service in GitHub. Blader naar uw GitHub-opslagplaats en gaat u naar **instellingen** > **toepassingen** > **OAuth-Apps toegestaan**. Selecteer **Azure App Service**, en selecteer vervolgens **verlenen**.
     
   - Selecteer de Bitbucket voor Bitbucket, **Team**, **opslagplaats**, en **vertakking** u wilt implementeren, voortdurend.
     
   - Voor Azure-opslagplaatsen, selecteert u de **Azure DevOps-organisatie**, **Project**, **opslagplaats**, en **vertakking** u wilt implementeren, voortdurend.
     
     > [!NOTE]
     > Als uw organisatie Azure DevOps niet wordt weergegeven, zorg er dan voor dat gekoppeld aan uw Azure-abonnement. Zie voor meer informatie, [een DevOps-Services van Azure-account instellen zodat deze op een web-app implementeren kunt](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)...
     
1. Selecteer **Doorgaan**.
   
   ![Gegevens van de opslagplaats in te vullen en vervolgens doorgaan.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Na het configureren van de build-provider, Controleer de instellingen op de **samenvatting** pagina en selecteer vervolgens **voltooien**.
   
   Nieuwe doorvoeringen in de geselecteerde opslagplaats en de vertakking nu implementeren continu in uw App Service-app. U kunt de doorvoeringen en implementaties volgen op de **Implementatiecentrum** pagina.
   
   ![Doorvoeringen en implementaties in Implementatiecentrum bijhouden](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-use-azure-pipelines"></a>Optie 2: Gebruik Azure Pipelines 

Als uw account de vereiste machtigingen beschikt heeft, kunt u Azure pijplijnen instellen continu implementeren vanuit GitHub of Azure-opslagplaatsen opslagplaatsen. Zie voor meer informatie over het implementeren via Azure pijplijnen [een web-app implementeren in Azure App Services](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

Voor Azure App Service voor het maken van continue levering Azure pijplijnen in uw organisatie Azure DevOps: 

- Uw Azure-account moet machtigingen voor het schrijven naar Azure Active Directory en het maken van een service hebben. 
  
- Uw Azure-account moet hebben de **eigenaar** rol in uw Azure-abonnement.

- U moet een beheerder in het Azure DevOps-project dat u wilt gebruiken.

Het configureren van Azure-pijplijnen (Preview):

1. Selecteer **App Services** in de [Azure-portal](https://portal.azure.com) linkernavigatiebalk en selecteer vervolgens de web-app die u wilt implementeren. 
   
1. Selecteer op de pagina app- **Implementatiecentrum** in het menu links.
   
1. Op de **Build-provider** weergeeft, schakelt **Azure pijplijnen (Preview)** , en selecteer vervolgens **doorgaan**. 
   
1. Op de **configureren** pagina, in de **Code** sectie:
   
   - Vervolgkeuzelijst voor GitHub, en selecteer de **organisatie**, **opslagplaats**, en **vertakking** u wilt implementeren, voortdurend.
     
     > [!NOTE]
     > Als u alle opslagplaatsen niet ziet, moet u mogelijk om te autoriseren Azure App Service in GitHub. Blader naar uw GitHub-opslagplaats en gaat u naar **instellingen** > **toepassingen** > **OAuth-Apps toegestaan**. Selecteer **Azure App Service**, en selecteer vervolgens **verlenen**.
     
   - Voor Azure-opslagplaatsen, selecteert u de **Azure DevOps-organisatie**, **Project**, **opslagplaats**, en **vertakking** u wilt implementeren, of configureer een nieuwe Azure DevOps-organisatie.
     
     > [!NOTE]
     > Als uw bestaande Azure DevOps-organisatie niet wordt weergegeven, moet u wellicht koppelen aan uw Azure-abonnement. Zie voor meer informatie, [definiëren van uw CD release-pijplijn](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
     
1. Selecteer **Doorgaan**.
   
1. Voor Azure-opslagplaatsen, in de **bouwen** sectie, geeft u het taalframework die Azure pijplijnen moet gebruiken voor het uitvoeren van build-taken, en selecteer vervolgens **doorgaan**.
   
1. Op de **Test** pagina, kies of u wilt inschakelen load tests uit en selecteer vervolgens **doorgaan**.
   
1. Afhankelijk van uw App Service-plan [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/), ziet u mogelijk een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](deploy-staging-slots.md), en selecteer vervolgens **doorgaan**.
   
   > [!NOTE]
   > Azure pijplijnen kunnen geen onafgebroken levering naar de productiesite. Deze beperking wordt voorkomen dat per ongeluk implementaties naar productie. Onafgebroken levering naar een staging-site instellen, Controleer of de gewenste wijzigingen en klikt u vervolgens de sleuven wisselen wanneer u klaar bent.
   
1. Na het configureren van de build-provider, Controleer de instellingen op de **samenvatting** pagina en selecteer vervolgens **voltooien**.
   
   Nieuwe doorvoeringen in de geselecteerde opslagplaats en de vertakking nu implementeren continu in uw App Service-app. U kunt de doorvoeringen en implementaties volgen op de **Implementatiecentrum** pagina.
   
   ![Doorvoeringen en implementaties in Implementatiecentrum bijhouden](media/app-service-continuous-deployment/github-finished.png)

## <a name="disable-continuous-deployment"></a>Continue implementatie uitschakelen

Als u continue implementatie uitschakelen, inschakelen **verbinding verbreken** aan de bovenkant van uw app **Implementatiecentrum** pagina.

![Continue implementatie uitschakelen](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Aanvullende resources

* [Veelvoorkomende problemen met doorlopende implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure PowerShell gebruiken](/powershell/azureps-cmdlets-docs)
* [Git-documentatie](https://git-scm.com/documentation)
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)

[Een repo maken (GitHub)]: https://help.github.com/articles/create-a-repo
[Een repo maken (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Maak een nieuwe Git-opslagplaats (Azure-opslagplaatsen)]: /azure/devops/repos/git/creatingrepo
