---
title: CI/CD met Azure Containerservice- en Swarm
description: Gebruik Azure Container Service met Docker Swarm, een Azure Container Registry en Visual Studio Team Services te leveren voortdurend een toepassing met meerdere containers .NET Core
services: container-service
author: jcorioland
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: ac3133ac093d578c89d24bddd1cc0a7c9588c2fd
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714995"
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Volledige CI/CD-pijplijn voor het implementeren van een toepassing met meerdere containers in Azure Container Service met Docker Swarm met Visual Studio Team Services

Een van de grootste uitdagingen bij het ontwikkelen van moderne toepassingen voor de cloud wordt deze toepassingen continu leveren. In dit artikel leert u hoe u een volledige continue integratie en implementatie (CI/CD)-pijplijn met behulp van Azure Container Service met Docker Swarm, Azure Container Registry en Visual Studio Team Services build implementeren en versiebeheer.

In dit artikel is gebaseerd op een eenvoudige toepassing, dat beschikbaar is op [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), ontwikkeld met ASP.NET Core. De toepassing bestaat uit vier verschillende services: drie web-API's en een web-front-end:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Het doel is deze toepassing voortdurend te leveren in een Docker Swarm-cluster met behulp van Visual Studio Team Services. De volgende afbeelding bevat deze pijplijn voor continue levering:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Hier volgt een korte beschrijving van de stappen uit:

1. Codewijzigingen zijn doorgevoerd in de opslagplaats van de broncode (in dit geval GitHub) 
1. GitHub activeert een build in Visual Studio Team Services 
1. Visual Studio Team Services haalt de meest recente versie van de bronnen en alle installatiekopieën waaruit de toepassing is gebaseerd 
1. Visual Studio Team Services elke installatiekopie naar een Docker-register dat is gemaakt met behulp van de service Azure Container Registry gepusht 
1. Visual Studio Team Services een nieuwe versie wordt geactiveerd 
1. De release van sommige opdrachten met behulp van SSH op het hoofdknooppunt van Azure container service-cluster wordt uitgevoerd 
1. Docker Swarm in het cluster haalt de meest recente versie van de installatiekopieën 
1. De nieuwe versie van de toepassing wordt geïmplementeerd met behulp van Docker Compose 

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u de volgende taken uitvoeren:

- [Een Swarm-cluster in Azure Container Service maken](container-service-deployment.md)
- [Verbinding maken met het Swarm-cluster in Azure Container Service](../container-service-connect.md)
- [Maak een Azure container registry](../../container-registry/container-registry-get-started-portal.md)
- [Een Visual Studio Team Services-account en team-project gemaakt](https://docs.microsoft.com/vsts/organizations/accounts/create-organization-msa-or-work-student)
- [Fork van de GitHub-opslagplaats naar uw GitHub-account](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

U moet ook een Ubuntu (14.04 of 16.04) virtuele machine met Docker is geïnstalleerd. Deze machine wordt door Visual Studio Team Services gebruikt tijdens de build- en -processen. Een manier om deze machine te maken is het gebruik van de installatiekopie die beschikbaar zijn in de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Stap 1: Uw Visual Studio Team Services-account configureren 

In deze sectie configureert u uw Visual Studio Team Services-account.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Een Linux van Visual Studio Team Services build-agent configureren

Docker-installatiekopieën maken en deze installatiekopieën naar een Azure container registry pushen van een Visual Studio Team Services-build, moet u een Linux-agent registreren. U hebt deze installatieopties:

* [Een agent op Linux implementeren](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Docker gebruiken om uit te voeren van de VSTS-agent](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a>De Docker-integratie VSTS-extensie installeren

Microsoft biedt een VSTS-extensie om te werken met Docker in build en release van processen. Deze uitbreiding is beschikbaar in de [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Klik op **installeren** deze extensie toevoegen aan uw VSTS-account:

![De Docker-integratie installeren](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

U wordt gevraagd of u verbinding maken met uw VSTS-account met uw referenties. 

### <a name="connect-visual-studio-team-services-and-github"></a>Verbinding maken met Visual Studio teamservices en GitHub

Instellen van een verbinding tussen uw VSTS-project en uw GitHub-account.

1. In het Visual Studio Team Services-project, klikt u op de **instellingen** pictogram in de werkbalk en selecteert u **Services**.

    ![Visual Studio teamservices - verbinding voor externe](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

1. Aan de linkerkant, klikt u op **nieuwe Service-eindpunt** > **GitHub**.

    ![Visual Studio teamservices - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

1. Als u wilt machtigen VSTS om te werken met uw GitHub-account, klikt u op **autoriseren** en volg de procedure in het venster dat wordt geopend.

    ![Visual Studio teamservices - autoriseren GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a>VSTS verbinden met uw Azure-containerregister en Azure Container Service-cluster

De laatste stappen voor het ophalen van in de CI/CD-pijplijn zijn voor het configureren van externe verbindingen naar het containerregister en de Docker Swarm-cluster in Azure. 

1. In de **Services** -instellingen van uw Visual Studio Team Services-project toevoegen een service-eindpunt van het type **Docker-register**. 

1. Voer de URL en de referenties van uw Azure-containerregister in het pop-upvenster dat wordt geopend.

    ![Visual Studio teamservices - Docker-register](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

1. Voor de Docker Swarm-cluster, voegt u toe een eindpunt van het type **SSH**. Voer vervolgens de SSH-verbindingsgegevens van de Swarm-cluster.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Alle de configuratie wordt nu uitgevoerd. In de volgende stappen maakt u de CI/CD-pijplijn die u bouwt en implementeert de toepassing op het Docker Swarm-cluster. 

## <a name="step-2-create-the-build-definition"></a>Stap 2: De build-definitie maken

In deze stap maakt u een build-definitionfor instellen met uw VSTS-project en de build-werkstroom voor uw containerinstallatiekopieën definiëren

### <a name="initial-definition-setup"></a>Installatie van de initiële definitie

1. Voor het maken van een build-definitie, verbinding maken met uw Visual Studio Team Services-project en klik op **Build & Release**. 

1. In de **bouwen definities** sectie, klikt u op **+ nieuw**. Selecteer de **leeg** sjabloon.

    ![Visual Studio teamservices - nieuwe Build-definitie](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

1. De nieuwe build configureren met een GitHub-opslagplaats bron, Controleer **continue integratie**, en selecteert u de agent-wachtrij waar u uw Linux-agent hebt geregistreerd. Klik op **maken** te maken van de build-definitie.

    ![Visual Studio teamservices - maken Build-definitie](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

1. Op de **bouwen definities** pagina, opent u eerst de **opslagplaats** tabblad en configureren van de build voor het gebruik van de fork van de MyShop-project dat u hebt gemaakt in de vereisten. Zorg ervoor dat u selecteert *acs-docs* als de **standaardvertakking**.

    ![Visual Studio teamservices - configuratie van de Build-opslagplaats](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

1. Op de **Triggers** tabblad, configureer dan de build wordt geactiveerd na elke doorvoer. Selecteer **continue integratie** en **wijzigingen voor Batch**.

    ![Visual Studio teamservices - configuratie van de Build-Trigger](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>De werkstroom build definiëren
De volgende stappen definiëren de build-werkstroom. Er zijn vijf containerinstallatiekopieën maken voor de *MyShop* toepassing. Elke installatiekopie wordt gemaakt met behulp van de docker-bestand zich in de projectmappen:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* Winkelpagina

U moet het toevoegen van twee Docker-stappen voor elke afbeelding, een om de installatiekopie te bouwen en een met de installatiekopie in Azure container registry hebt gepusht. 

1. Als u wilt toevoegen een stap in de build-werkstroom, klikt u op **+ Add build step** en selecteer **Docker**.

    ![Visual Studio teamservices - toevoegen Build-stappen](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

1. Configureren van elke afbeelding, een stap die gebruikmaakt van de `docker build` opdracht.

    ![Visual Studio teamservices - Docker-Build](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Voor de bewerking voor het samenstellen, selecteert u uw Azure-containerregister, de **bouw een installatiekopie** actie en de Dockerfile die definieert van elke afbeelding. Stel de **Build context** als het bestand Dockerfile-hoofdmap en definieer de **installatiekopie met de naam**. 
    
    Zoals in het vorige scherm wordt weergegeven, start u de naam van de installatiekopie met de URI van uw Azure container registry. (U kunt ook een build-variabele gebruiken om te voorzien van de code van de installatiekopie, zoals de build-id in dit voorbeeld.)

1. Voor elke afbeelding, configureert u een tweede stap die gebruikmaakt van de `docker push` opdracht.

    ![Visual Studio teamservices - Docker Push](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Voor de push-bewerking, selecteert u uw Azure-containerregister, de **een installatiekopie pushen** actie, en voer de **Installatiekopienaam** die is ingebouwd in de vorige stap.

1. Nadat u de build- en push-stappen voor elk van de vijf installatiekopieën hebt geconfigureerd, moet u twee meer stappen toevoegen in de build-werkstroom.

    a. Een opdrachtregeltaak die gebruikmaakt van een bash-script om te vervangen de *BuildNumber* exemplaar in de docker-compose.yml-bestand met de huidige id bouwen Zie het volgende scherm voor meer informatie.

    ![Visual Studio teamservices - Update-Compose-bestand](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Een taak die het bijgewerkte opstellen-bestand als een build-artefact komt, zodat deze kan worden gebruikt in de release. Zie het volgende scherm voor meer informatie.

    ![Visual Studio Team Services - publiceren opstellen bestand](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

1. Klik op **opslaan** en de naam van uw build-definitie.

## <a name="step-3-create-the-release-definition"></a>Stap 3: De release-definitie maken

Visual Studio Team Services kunt u [versies beheren in omgevingen](https://www.visualstudio.com/team-services/release-management/). U kunt continue implementatie om ervoor te zorgen dat uw toepassing wordt geïmplementeerd op de verschillende omgevingen (zoals ontwikkelen, testen, Pre-productie en productie) in een goede manier inschakelen. U kunt een nieuwe omgeving die staat voor uw Azure Container Service Docker Swarm-cluster maken.

![Visual Studio teamservices - versie naar ACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Installatie van de eerste release

1. Klik op om een versiedefinitie **Releases** > **+ Release**

1. Als u wilt de artefact configureren, klikt u op **artefacten** > **een artefact koppelingsbron**. Hier, koppelt u deze nieuwe release-definitie aan de build die u hebt gedefinieerd in de vorige stap. Op deze manier is de docker-compose.yml-bestand beschikbaar in het uitgifteproces.

    ![Visual Studio teamservices - Release artefacten](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

1. Voor het configureren van de release-trigger, klikt u op **Triggers** en selecteer **continue implementatie**. De trigger instellen op de dezelfde bron-artefact. Deze instelling zorgt ervoor dat er een nieuwe versie wordt gestart zodra de build voltooid is.

    ![Visual Studio teamservices - Release-Triggers](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>De release-werkstroom definiëren

De release-werkstroom bestaat uit twee taken die u toevoegt.

1. Configureren van een taak voor het veilig kopiëren van het opstellen-bestand naar een *implementeren* map op de Docker Swarm-hoofdknooppunt, met behulp van de SSH-verbinding die u eerder hebt geconfigureerd. Zie het volgende scherm voor meer informatie.

    ![Visual Studio teamservices - Release SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

1. Configureren van een tweede taak voor het uitvoeren van een bash-opdracht uit te voeren `docker` en `docker-compose` opdrachten op het hoofdknooppunt. Zie het volgende scherm voor meer informatie.

    ![Visual Studio teamservices - Release Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    De opdracht uitgevoerd op de master gebruiken de Docker CLI en de CLI Docker-Compose hiervoor de volgende taken:

    - Meld u aan bij Azure container registry (hierbij drie build variab'les die zijn gedefinieerd in de **variabelen** tabblad)
    - Definieer de **DOCKER_HOST** variabele om te werken met het Swarm-eindpunt (: 2375)
    - Navigeer naar de *implementeren* map die is gemaakt met de vorige beveiligde kopie-taak en die de docker-compose.yml-bestand bevat 
    - Voer `docker-compose` opdrachten die ophalen van de nieuwe installatiekopieën, stop de services, verwijder de services en de containers te maken.

    >[!IMPORTANT]
    > Zoals in het vorige scherm wordt weergegeven, laat de **mislukken op STDERR** selectievakje uitgeschakeld. Dit is een belangrijk instelling, omdat `docker-compose` worden afgedrukt verschillende diagnostische berichten, zoals containers zijn moet worden gestopt of wordt verwijderd, klikt u op de standaardfout-uitvoer. Als u het selectievakje Visual Studio Team Services-rapporten dat fouten zijn opgetreden tijdens de release, zelfs als het goed.
    >
1. Sla deze nieuwe release-definitie.


>[!NOTE]
>Deze implementatie bevat enige uitvaltijd, omdat we stoppen van de oude services en de nieuwe computer uitgevoerd. Het is mogelijk om dit te voorkomen dat aan de hand van een blue-green implementatie.
>

## <a name="step-4-test-the-cicd-pipeline"></a>Stap 4. De CI/CD-pijplijn testen

Nu u klaar bent met de configuratie, is het tijd om te testen van deze nieuwe CI/CD-pijplijn. De eenvoudigste manier om dit te testen, is voor het bijwerken van de broncode en de wijzigingen aan in uw GitHub-opslagplaats. Een paar seconden nadat u de code hebt gepusht, ziet u een nieuwe build in Visual Studio Team Services wordt uitgevoerd. Eenmaal is voltooid, wordt een nieuwe versie wordt geactiveerd en wordt de nieuwe versie van de toepassing op het Azure Container Service-cluster implementeren.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over CI/CD met Visual Studio Team Services, de [VSTS Build-overzicht](https://www.visualstudio.com/docs/build/overview).
