---
title: Continue implementatie in Azure App Service | Microsoft Docs
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
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: 4d3f1c66c6403720bf02c80af1d6833dc3cee3f1
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "42058366"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continue implementatie in Azure App Service
Dit artikel leest u hoe u continue implementatie voor [Azure App Service](app-service-web-overview.md). App Service kunt u continue implementatie vanuit BitBucket, GitHub, en [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) door te halen in de meest recente updates van uw bestaande opslagplaats in een van deze services.

Voor meer informatie over het configureren van continue implementatie handmatig vanuit de opslagplaats van een cloud niet vermeld in de Azure-portal (zoals [GitLab](https://gitlab.com/)), Zie [instellen van continue implementatie met behulp van handmatige stappen](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

De voorbereide-opslagplaats publiceren naar een van de ondersteunde services. Zie [Een repo maken (GitHub)], [Een repo maken (BitBucket)] en [Aan de slag met VSTS] voor meer informatie over het publiceren van uw project bij deze services.

## <a name="deploy-continuously-from-github"></a>Doorlopend implementeren vanuit GitHub

Om in te schakelen doorlopende implementatie met GitHub, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **GitHub** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-continuous-deployment/github-choose-source.png)

U hoeft alleen te autoriseren met GitHub één keer. Als u al bent gemachtigd, klikt u op **doorgaan**. U kunt het geautoriseerde GitHub-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-continuous-deployment/github-continue.png)

In de **Build-provider** pagina, kies de build-provider en klikt u op > **doorgaan**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Optie 1: gebruik die App Service Kudu server maken

In de **configureren** pagina, selecteert u de organisatie, de opslagplaats en het vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **doorgaan**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Optie 2: VSTS constante levering gebruiken

> [!NOTE]
> Voor App Service voor het maken van de benodigde build en release-definities in uw VSTS-account, moet uw Azure-account de rol van **eigenaar** in uw Azure-abonnement.
>

In de **configureren** pagina, in de **Code** sectie, selecteer de organisatie, de opslagplaats en het vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **doorgaan**.

In de **configureren** pagina, in de **bouwen** sectie, configureren van een nieuwe VSTS-account of een bestaand account opgeven. Wanneer u klaar bent, klikt u op **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaande VSTS-account dat niet wordt vermeld wilt, moet u [het VSTS-account koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **Test** pagina, kies of u wilt inschakelen load tests uit en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/) van uw App Service-plan, ziet u mogelijk ook een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](web-sites-staged-publishing.md), klikt u vervolgens op **doorgaan**.

### <a name="finish-configuration"></a>Configuratie voltooien

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Wanneer de configuratie is voltooid, worden voortdurend nieuwe doorvoeringen in de geselecteerde opslagplaats geïmplementeerd in uw App Service-app.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Doorlopend implementeren in BitBucket

Om in te schakelen doorlopende implementatie met BitBucket, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **BitBucket** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

U hoeft alleen te autoriseren met BitBucket één keer. Als u al bent gemachtigd, klikt u op **doorgaan**. U kunt de geautoriseerde BitBucket-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

In de **configureren** pagina, selecteert u de opslagplaats en de vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **doorgaan**.

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Wanneer de configuratie is voltooid, worden voortdurend nieuwe doorvoeringen in de geselecteerde opslagplaats geïmplementeerd in uw App Service-app.

## <a name="deploy-continuously-from-vsts"></a>Continu implementeren vanuit VSTS

Om in te schakelen doorlopende implementatie met VSTS, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **VSTS** > **doorgaan**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

In de **Build-provider** pagina, kies de build-provider en klikt u op > **doorgaan**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Optie 1: gebruik die App Service Kudu server maken

In de **configureren** pagina, selecteert u de VSTS-account, project-opslagplaats en vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **doorgaan**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Optie 2: VSTS constante levering gebruiken

> [!NOTE]
> Voor App Service voor het maken van de benodigde build en release-definities in uw VSTS-account, moet uw Azure-account de rol van **eigenaar** in uw Azure-abonnement.
>

In de **configureren** pagina, in de **Code** sectie, selecteer de VSTS-account, project-opslagplaats en vertakking waaruit u wilt continue implementatie. Wanneer u klaar bent, klikt u op **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaande VSTS-account dat niet wordt vermeld wilt, moet u [het VSTS-account koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **configureren** pagina, in de **bouwen** sectie, geeft u het taalframework die VSTS gebruiken moeten voor het uitvoeren van de build-taken voor de geselecteerde opslagplaats. Wanneer u klaar bent, klikt u op **doorgaan**.

In de **Test** pagina, kies of u wilt inschakelen load tests uit en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/) van uw App Service-plan, ziet u mogelijk ook een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](web-sites-staged-publishing.md), klikt u vervolgens op **doorgaan**. 

### <a name="finish-configuration"></a>Configuratie voltooien

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Wanneer de configuratie is voltooid, worden voortdurend nieuwe doorvoeringen in de geselecteerde opslagplaats geïmplementeerd in uw App Service-app.

## <a name="disable-continuous-deployment"></a>Continue implementatie uitschakelen

Voor continue implementatie uitschakelen, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **GitHub** of **VSTS** of **BitBucket**  >  **Verbreken**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Aanvullende resources

* [Veelvoorkomende problemen met continue implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [PowerShell voor Azure gebruiken]
* [Git-documentatie]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Gebruik Azure voor het automatisch genereren van een CI/CD-pijplijn voor het implementeren van een ASP.NET 4-app](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[PowerShell voor Azure gebruiken]: /powershell/azureps-cmdlets-docs
[Git-documentatie]: http://git-scm.com/documentation

[Een repo maken (GitHub)]: https://help.github.com/articles/create-a-repo
[Een repo maken (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Aan de slag met VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
