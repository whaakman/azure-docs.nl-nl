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
ms.date: 12/03/2018
ms.author: cephalin;dariagrigoriu
ms.custom: seodec18
ms.openlocfilehash: 1313616818686c7a03269fc1cc837958665732d8
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725232"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continue implementatie in Azure App Service
Dit artikel leest u hoe u continue implementatie voor [Azure App Service](overview.md). App Service kunt u continue implementatie vanuit BitBucket, GitHub, en [Azure DevOps Services](https://www.visualstudio.com/team-services/) door te halen in de meest recente updates van uw bestaande opslagplaats in een van deze services.

Voor meer informatie over het configureren van continue implementatie handmatig vanuit de opslagplaats van een cloud niet vermeld in de Azure-portal (zoals [GitLab](https://gitlab.com/)), Zie [instellen van continue implementatie met behulp van handmatige stappen](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

De voorbereide-opslagplaats publiceren naar een van de ondersteunde services. Zie voor meer informatie over het publiceren van uw project met deze services [een repo maken (GitHub)], [een repo maken (BitBucket)], en [aan de slag met Azure DevOps-Services].

## <a name="deploy-continuously-from-github"></a>Doorlopend implementeren vanuit GitHub

Om in te schakelen doorlopende implementatie met GitHub, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **GitHub** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-continuous-deployment/github-choose-source.png)

U hoeft alleen te autoriseren met GitHub één keer. Als u al bent gemachtigd, klikt u op **Doorgaan**. U kunt het geautoriseerde GitHub-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-continuous-deployment/github-continue.png)

In de **Build-provider** pagina, kies de build-provider en klikt u op > **doorgaan**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Optie 1: gebruik die App Service Kudu server maken

In de **configureren** pagina, selecteert u de organisatie, de opslagplaats en het vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **Doorgaan**.

### <a name="option-2-use-azure-pipelines-preview"></a>Optie 2: gebruik van Azure-pijplijnen (preview)

> [!NOTE]
> Voor App Service voor het maken van de benodigde Azure-pijplijnen in uw organisatie Azure DevOps-Services, moet uw Azure-account de rol van **eigenaar** in uw Azure-abonnement.
>

In de **configureren** pagina, in de **Code** sectie, selecteer de organisatie, de opslagplaats en het vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **Doorgaan**.

In de **configureren** pagina, in de **bouwen** sectie, een nieuwe organisatie van Azure DevOps-Services configureren of een bestaande organisatie opgeven. Wanneer u klaar bent, klikt u op **Doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaande organisatie van Azure DevOps-Services die niet wordt vermeld wilt, moet u [de Services van Azure DevOps-organisatie koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **Test** pagina, kies of u wilt inschakelen load tests uit en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/) van uw App Service-plan, ziet u mogelijk ook een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](deploy-staging-slots.md), klikt u vervolgens op **doorgaan**.

### <a name="finish-configuration"></a>Configuratie voltooien

In de **Samenvatting** pagina, controleer uw opties en klik op **Voltooien**.

Wanneer de configuratie is voltooid, worden voortdurend nieuwe doorvoeringen in de geselecteerde opslagplaats geïmplementeerd in uw App Service-app.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Doorlopend implementeren in BitBucket

Om in te schakelen doorlopende implementatie met BitBucket, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **BitBucket** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

U hoeft alleen te autoriseren met BitBucket één keer. Als u al bent gemachtigd, klikt u op **Doorgaan**. U kunt de geautoriseerde BitBucket-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

In de **configureren** pagina, selecteert u de opslagplaats en de vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **Doorgaan**.

In de **Samenvatting** pagina, controleer uw opties en klik op **Voltooien**.

Wanneer de configuratie is voltooid, worden voortdurend nieuwe doorvoeringen in de geselecteerde opslagplaats geïmplementeerd in uw App Service-app.

## <a name="deploy-continuously-from-azure-repos-devops-services"></a>Continu implementeren vanuit Azure-opslagplaatsen (DevOps-Services)

Inschakelen van continue implementatie met vanuit [Azure opslagplaatsen](https://docs.microsoft.com/azure/devops/repos/index), gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **Azure opslagplaatsen** > **doorgaan**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

In de **Build-provider** pagina, kies de build-provider en klikt u op > **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaande organisatie van Azure DevOps-Services die niet wordt vermeld wilt, moet u [de Services van Azure DevOps-organisatie koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

### <a name="option-1-use-app-service-kudu-build-server"></a>Optie 1: gebruik die App Service Kudu server maken

In de **configureren** pagina, selecteert u de organisatie van Azure DevOps-Services, project-opslagplaats en vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **Doorgaan**.

### <a name="option-2-use-azure-devops-services-continuous-delivery"></a>Optie 2: gebruik continue levering van Azure DevOps-Services

> [!NOTE]
> Voor App Service voor het maken van de benodigde Azure-pijplijnen in uw organisatie Azure DevOps-Services, moet uw Azure-account de rol van **eigenaar** in uw Azure-abonnement.
>

In de **configureren** pagina, in de **Code** sectie, selecteer de organisatie van Azure DevOps-Services, project-opslagplaats en vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **Doorgaan**.

In de **configureren** pagina, in de **bouwen** sectie, geeft u het taalframework die Azure DevOps-Services gebruiken moeten voor het uitvoeren van de build-taken voor de geselecteerde opslagplaats. Wanneer u klaar bent, klikt u op **Doorgaan**.

In de **Test** pagina, kies of u wilt inschakelen load tests uit en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/) van uw App Service-plan, ziet u mogelijk ook een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](deploy-staging-slots.md), klikt u vervolgens op **doorgaan**. 

### <a name="finish-configuration"></a>Configuratie voltooien

In de **Samenvatting** pagina, controleer uw opties en klik op **Voltooien**.

Wanneer de configuratie is voltooid, worden voortdurend nieuwe doorvoeringen in de geselecteerde opslagplaats geïmplementeerd in uw App Service-app.

## <a name="disable-continuous-deployment"></a>Continue implementatie uitschakelen

Voor continue implementatie uitschakelen, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **GitHub** of **Azure DevOps Services** of **BitBucket**  >  **Verbreken**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Aanvullende resources

* [Veelvoorkomende problemen met continue implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [PowerShell voor Azure gebruiken]
* [Git-documentatie]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Gebruik Azure voor het automatisch genereren van een CI/CD-pijplijn voor het implementeren van een ASP.NET 4-app](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[Azure DevOps portal]: https://azure.microsoft.com/services/devops/
[Installing Git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
[PowerShell voor Azure gebruiken]: /powershell/azureps-cmdlets-docs
[Git-documentatie]: https://git-scm.com/documentation

[Een repo maken (GitHub)]: https://help.github.com/articles/create-a-repo
[Een repo maken (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Aan de slag met Azure DevOps-Services]: https://docs.microsoft.com/azure/devops/user-guide/devops-alm-overview
