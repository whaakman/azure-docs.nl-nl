---
title: Continue implementatie naar Azure App Service | Microsoft Docs
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
ms.openlocfilehash: e587edeef1cfa080a81f523f63678a645b514c57
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849488"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continue implementatie naar Azure App Service
Dit artikel ziet u het configureren van continue implementatie voor [Azure App Service](app-service-web-overview.md). App Service kunt continue implementatie van BitBucket, GitHub en [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) door binnen te halen in de meest recente updates van uw bestaande opslagplaats in een van deze services.

Voor informatie over het configureren van continue implementatie handmatig van een cloud-opslagplaats niet vermeld in de Azure-portal (zoals [GitLab](https://gitlab.com/)), Zie [instellen van continue implementatie met handmatige stappen](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Uw opslagplaats voorbereid op een van de ondersteunde services publiceren. Zie [Een repo maken (GitHub)], [Een repo maken (BitBucket)] en [Aan de slag met VSTS] voor meer informatie over het publiceren van uw project bij deze services.

## <a name="deploy-continuously-from-github"></a>Continu implementeren vanuit GitHub

Om continue implementatie met GitHub, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **GitHub** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-continuous-deployment/github-choose-source.png)

U hoeft alleen te autoriseren eenmaal met GitHub. Als u al bent gemachtigd, klikt u op **doorgaan**. U kunt de geautoriseerde GitHub-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-continuous-deployment/github-continue.png)

In de **Build-provider** pagina, kies de build-provider en klikt u op > **doorgaan**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Optie 1: App Service Kudu build-server gebruiken

In de **configureren** pagina, selecteert u de organisatie, de opslagplaats en het vertakking waaruit u wilt implementeren, voortdurend. Wanneer u klaar bent, klikt u op **doorgaan**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Optie 2: VSTS continue levering gebruiken

> [!NOTE]
> Voor App Service voor het maken van de benodigde build en definities in uw account VSTS release, uw Azure-account de rol van moet zijn **eigenaar** in uw Azure-abonnement.
>

In de **configureren** pagina in de **Code** sectie, selecteert u de organisatie, de opslagplaats en het vertakking waaruit u wilt implementeren, voortdurend. Wanneer u klaar bent, klikt u op **doorgaan**.

In de **configureren** pagina in de **bouwen** sectie, configureert u een nieuw VSTS-account of geef een bestaande account. Wanneer u klaar bent, klikt u op **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaand VSTS account die niet wordt vermeld wilt, moet u [de VSTS-account koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **Test** pagina, kies of u wilt laden tests inschakelen en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](/pricing/details/app-service/plans/) van uw App Service-abonnement u ziet misschien ook een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](web-sites-staged-publishing.md), klikt u vervolgens op **doorgaan**.

### <a name="finish-configuration"></a>Configuratie voltooien

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Wanneer de configuratie is voltooid, worden nieuwe doorvoeringen in de geselecteerde opslagplaats continu geïmplementeerd in uw App Service-app.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Continu implementeren vanaf BitBucket

Om continue implementatie met BitBucket, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **BitBucket** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

U hoeft alleen te autoriseren eenmaal met BitBucket. Als u al bent gemachtigd, klikt u op **doorgaan**. U kunt het geautoriseerde BitBucket-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

In de **configureren** pagina, selecteert u de opslagplaats en de vertakking waaruit u wilt implementeren, voortdurend. Wanneer u klaar bent, klikt u op **doorgaan**.

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Wanneer de configuratie is voltooid, worden nieuwe doorvoeringen in de geselecteerde opslagplaats continu geïmplementeerd in uw App Service-app.

## <a name="deploy-continuously-from-vsts"></a>Continu implementeren vanaf VSTS

Om continue implementatie met VSTS, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **VSTS** > **doorgaan**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

In de **Build-provider** pagina, kies de build-provider en klikt u op > **doorgaan**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Optie 1: App Service Kudu build-server gebruiken

In de **configureren** pagina, selecteert u de VSTS account, project-opslagplaats en vertakking waaruit u wilt implementeren, voortdurend. Wanneer u klaar bent, klikt u op **doorgaan**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Optie 2: VSTS continue levering gebruiken

> [!NOTE]
> Voor App Service voor het maken van de benodigde build en definities in uw account VSTS release, uw Azure-account de rol van moet zijn **eigenaar** in uw Azure-abonnement.
>

In de **configureren** pagina in de **Code** sectie, selecteert u de VSTS account, project-opslagplaats en vertakking waaruit u wilt implementeren, voortdurend. Wanneer u klaar bent, klikt u op **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaand VSTS account die niet wordt vermeld wilt, moet u [de VSTS-account koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **configureren** pagina in de **bouwen** sectie, geeft u het taalframework die VSTS gebruiken moeten voor het uitvoeren van de build-taken voor de geselecteerde opslagplaats. Wanneer u klaar bent, klikt u op **doorgaan**.

In de **Test** pagina, kies of u wilt laden tests inschakelen en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](/pricing/details/app-service/plans/) van uw App Service-abonnement u ziet misschien ook een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](web-sites-staged-publishing.md), klikt u vervolgens op **doorgaan**. 

### <a name="finish-configuration"></a>Configuratie voltooien

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Wanneer de configuratie is voltooid, worden nieuwe doorvoeringen in de geselecteerde opslagplaats continu geïmplementeerd in uw App Service-app.

## <a name="disable-continuous-deployment"></a>Doorlopende implementatie uitschakelen

Als u wilt uitschakelen continue implementatie, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **GitHub** of **VSTS** of **BitBucket**  >  **Verbreken**.

![](media/app-service-continuous-deployment/disable.png)

## <a name="additional-resources"></a>Aanvullende resources

* [Veelvoorkomende problemen met continue implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [PowerShell voor Azure gebruiken]
* [Git-documentatie]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Gebruik Azure voor het automatisch genereren van een pijplijn CI/CD voor het implementeren van een app ASP.NET 4](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[PowerShell voor Azure gebruiken]: /powershell/azureps-cmdlets-docs
[Git-documentatie]: http://git-scm.com/documentation

[Een repo maken (GitHub)]: https://help.github.com/articles/create-a-repo
[Een repo maken (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Aan de slag met VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
