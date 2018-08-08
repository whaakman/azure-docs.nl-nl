---
title: CI/CD met Azure Container Service Engine en Swarm-modus
description: Azure Container Service Engine met Docker Swarm Mode, een Azure Container Registry en Visual Studio Team Services te leveren voortdurend een toepassing met meerdere containers .NET Core gebruiken
services: container-service
author: diegomrtnzg
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 6c41156383791fb7d72ac02dae919a25a0d15c84
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621059"
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a>Volledige CI/CD-pijplijn voor het implementeren van een toepassing met meerdere containers in Azure Container Service met ACS-Engine en Docker Swarm-modus met behulp van Visual Studio Team Services

*In dit artikel is gebaseerd op [volledige CI/CD-pijplijn voor het implementeren van een toepassing met meerdere containers in Azure Container Service met Docker Swarm met Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentatie*

Een van de grootste uitdagingen bij het ontwikkelen van moderne toepassingen voor de cloud wordt tegenwoordig, kunnen deze toepassingen continu leveren. In dit artikel leert u hoe u een volledige continue integratie en implementatie (CI/CD) met behulp van pijplijn implementeren: 
* Azure Container Service-Engine met Docker Swarm-modus
* Azure Container Registry
* Visual Studio Team Services

In dit artikel is gebaseerd op een eenvoudige toepassing, dat beschikbaar is op [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), ontwikkeld met ASP.NET Core. De toepassing bestaat uit vier verschillende services: drie web-API's en een web-front-end:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

Het doel is deze toepassing voortdurend te leveren in een Docker Swarm Mode-cluster met behulp van Visual Studio Team Services. De volgende afbeelding bevat deze pijplijn voor continue levering:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Hier volgt een korte beschrijving van de stappen uit:

1. Codewijzigingen zijn doorgevoerd in de opslagplaats van de broncode (in dit geval GitHub) 
2. GitHub activeert een build in Visual Studio Team Services 
3. Visual Studio Team Services haalt de meest recente versie van de bronnen en alle installatiekopieën waaruit de toepassing is gebaseerd 
4. Visual Studio Team Services elke installatiekopie naar een Docker-register dat is gemaakt met behulp van de service Azure Container Registry gepusht 
5. Visual Studio Team Services een nieuwe versie wordt geactiveerd 
6. De release van sommige opdrachten met behulp van SSH op het hoofdknooppunt van Azure container service-cluster wordt uitgevoerd 
7. Docker Swarm Mode in het cluster haalt de meest recente versie van de installatiekopieën 
8. De nieuwe versie van de toepassing wordt geïmplementeerd met behulp van Docker Stack 

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u de volgende taken uitvoeren:

- [Een Swarm Mode-cluster maken in Azure Container Service met de ACS-Engine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Verbinding maken met het Swarm-cluster in Azure Container Service](../container-service-connect.md)
- [Maak een Azure container registry](../../container-registry/container-registry-get-started-portal.md)
- [Een Visual Studio Team Services-account en team-project gemaakt](https://docs.microsoft.com/vsts/organizations/accounts/create-organization-msa-or-work-student)
- [Fork van de GitHub-opslagplaats naar uw GitHub-account](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> De Docker Swarm-orchestrator in Azure Container Service maakt gebruik van legacy standalone Swarm. Momenteel is de geïntegreerde [Swarm-modus](https://docs.docker.com/engine/swarm/) (in Docker 1.12 en hoger) geen ondersteunde orchestrator in Azure Container Service. Daarom gebruiken we [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), een door de community bijgedragen [quickstart-sjabloon](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), of een Docker-oplossing in de [Azure Marketplace](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Stap 1: Uw Visual Studio Team Services-account configureren 

In deze sectie configureert u uw Visual Studio Team Services-account. Als u wilt configureren VSTS-Services-eindpunten in uw Visual Studio Team Services-project, klikt u op de **instellingen** pictogram in de werkbalk en selecteert u **Services**.

![Open Services-eindpunt](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a>Verbinding maken met Visual Studio Team Services en Azure-account

Instellen van een verbinding tussen uw VSTS-project en uw Azure-account.

1. Aan de linkerkant, klikt u op **nieuwe Service-eindpunt** > **Azure Resource Manager**.
2. Als u wilt machtigen VSTS om te werken met uw Azure-account, selecteert u uw **abonnement** en klikt u op **OK**.

    ![Visual Studio teamservices - autoriseren Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a>Verbinding maken met Visual Studio teamservices en GitHub

Instellen van een verbinding tussen uw VSTS-project en uw GitHub-account.

1. Aan de linkerkant, klikt u op **nieuwe Service-eindpunt** > **GitHub**.
2. Als u wilt machtigen VSTS om te werken met uw GitHub-account, klikt u op **autoriseren** en volg de procedure in het venster dat wordt geopend.

    ![Visual Studio teamservices - autoriseren GitHub](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-to-your-azure-container-service-cluster"></a>VSTS verbinden met uw Azure Container Service-cluster

De laatste stappen voor het ophalen van in de CI/CD-pijplijn zijn voor het configureren van externe verbindingen met uw Docker Swarm-cluster in Azure. 

1. Voor de Docker Swarm-cluster, voegt u toe een eindpunt van het type **SSH**. Voer vervolgens de SSH-verbindingsgegevens van de Swarm-cluster (master knooppunt).

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Alle de configuratie wordt nu uitgevoerd. In de volgende stappen maakt u de CI/CD-pijplijn die u bouwt en implementeert de toepassing op het Docker Swarm-cluster. 

## <a name="step-2-create-the-build-definition"></a>Stap 2: De build-definitie maken

In deze stap maakt u een build-definitie voor uw VSTS-project instellen en definiëren van de build-werkstroom voor uw containerinstallatiekopieën

### <a name="initial-definition-setup"></a>Installatie van de initiële definitie

1. Voor het maken van een build-definitie, verbinding maken met uw Visual Studio Team Services-project en klik op **Build & Release**. In de **bouwen definities** sectie, klikt u op **+ nieuw**. 

    ![Visual Studio teamservices - nieuwe Build-definitie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Selecteer de **leeg proces**.

    ![Visual Studio teamservices - nieuwe, lege Build-definitie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Klik vervolgens op de **variabelen** tabblad en twee nieuwe variabelen maken: **RegistryURL** en **AgentURL**. Plak de waarden van uw register en de DNS-Cluster-Agents.

    ![Visual Studio teamservices - configuratie van de Build-variabelen](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. Op de **bouwen definities** pagina, open de **Triggers** tabblad en configureren van de build voor het gebruik van continue integratie met de fork van de MyShop-project dat u hebt gemaakt in de vereisten. Selecteer **wijzigingen voor Batch**. Zorg ervoor dat u selecteert *docker-linux* als de **vertakking specificatie**.

    ![Visual Studio teamservices - configuratie van de Build-opslagplaats](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Ten slotte klikt u op de **opties** tabblad en configureren van de wachtrij-agent naar **gehost Linux Preview**.

    ![Visual Studio teamservices - configuratie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a>De werkstroom build definiëren
De volgende stappen definiëren de build-werkstroom. Eerst moet u de bron van de code configureren. Om dat te doen, selecteert u **GitHub** en uw **opslagplaats** en **vertakking** (docker-linux).

![Configureren van Visual Studio teamservices - broncode](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Er zijn vijf containerinstallatiekopieën maken voor de *MyShop* toepassing. Elke installatiekopie wordt gemaakt met behulp van de docker-bestand zich in de projectmappen:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* Winkelpagina

U moet twee Docker-stappen voor elke afbeelding, een om de installatiekopie te bouwen en een met de installatiekopie in Azure container registry hebt gepusht. 

1. Als u wilt toevoegen een stap in de build-werkstroom, klikt u op **+ Add build step** en selecteer **Docker**.

    ![Visual Studio teamservices - toevoegen Build-stappen](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Configureren van elke afbeelding, een stap die gebruikmaakt van de `docker build` opdracht.

    ![Visual Studio teamservices - Docker-Build](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Voor de bewerking voor het samenstellen, selecteert u uw Azure Container Registry, de **bouw een installatiekopie** actie en de Dockerfile die definieert van elke afbeelding. Stel de **werkmap** als de hoofdmap van de docker-bestand definieert de **installatiekopie met de naam**, en selecteer **meest recente code opnemen**.
    
    Naam van de installatiekopie moet worden in deze indeling: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Vervang **[NAME]** met de naam van de installatiekopie:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Voor elke afbeelding, configureert u een tweede stap die gebruikmaakt van de `docker push` opdracht.

    ![Visual Studio teamservices - Docker Push](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Voor de push-bewerking, selecteert u uw Azure-containerregister, de **een installatiekopie pushen** actie, voer de **Installatiekopienaam** die is ingebouwd in de vorige stap en selecteer **meest recente code opnemen**.

4. Nadat u de build- en push-stappen voor elk van de vijf installatiekopieën hebt geconfigureerd, moet u drie meer stappen toevoegen in de build-werkstroom.

   ![Visual Studio teamservices - toevoegen opdrachtregel-taak](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. Een opdrachtregeltaak die gebruikmaakt van een bash-script om te vervangen de *RegistryURL* exemplaar in de docker-compose.yml-bestand met de variabele RegistryURL. 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio teamservices - Update-Compose-bestand met de Register-URL](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. Een opdrachtregeltaak die gebruikmaakt van een bash-script om te vervangen de *AgentURL* exemplaar in de docker-compose.yml-bestand met de variabele AgentURL.
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. Een taak die het bijgewerkte opstellen-bestand als een build-artefact komt, zodat deze kan worden gebruikt in de release. Zie het volgende scherm voor meer informatie.

         ![Visual Studio teamservices - publiceren artefact](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - publiceren opstellen bestand](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Klik op **opslaan en in de wachtrij** voor het testen van uw build-definitie.

   ![Visual Studio teamservices - opslaan en wachtrij](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio teamservices - nieuwe wachtrij](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Als de **bouwen** juist is, moet u dit scherm ziet:

  ![Visual Studio teamservices - Build is voltooid](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-definition"></a>Stap 3: De release-definitie maken

Visual Studio Team Services kunt u [versies beheren in omgevingen](https://www.visualstudio.com/team-services/release-management/). U kunt continue implementatie om ervoor te zorgen dat uw toepassing wordt geïmplementeerd op de verschillende omgevingen (zoals ontwikkelen, testen, Pre-productie en productie) in een goede manier inschakelen. U kunt een omgeving die staat voor uw Azure Container Service Docker Swarm Mode-cluster maken.

![Visual Studio teamservices - versie naar ACS](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Installatie van de eerste release

1. Klik op om een versiedefinitie **Releases** > **+ Release**

2. Als u wilt de artefact configureren, klikt u op **artefacten** > **een artefact koppelingsbron**. Hier, koppelt u deze nieuwe release-definitie aan de build die u hebt gedefinieerd in de vorige stap. Hierna is de docker-compose.yml-bestand beschikbaar in de release-proces.

    ![Visual Studio teamservices - Release artefacten](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. Voor het configureren van de release-trigger, klikt u op **Triggers** en selecteer **continue implementatie**. De trigger instellen op de dezelfde bron-artefact. Deze instelling zorgt ervoor dat er een nieuwe versie wordt gestart wanneer de build voltooid is.

    ![Visual Studio teamservices - Release-Triggers](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Voor het configureren van de release-variabelen, klikt u op **variabelen** en selecteer **+ variabele** drie nieuwe variabelen maken met de gegevens van het register: **docker.username**, **docker.password**, en **docker.registry**. Plak de waarden van uw register en de DNS-Cluster-Agents.

    ![Visual Studio teamservices - configuratie van de Build-opslagplaats](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Zoals in het vorige scherm wordt weergegeven, klikt u op de **vergrendeling** selectievakje in docker.password. Deze instelling is belangrijk om te beperken van het wachtwoord.
    >

### <a name="define-the-release-workflow"></a>De release-werkstroom definiëren

De release-werkstroom bestaat uit twee taken die u toevoegt.

1. Configureren van een taak voor het veilig kopiëren van het opstellen-bestand naar een *implementeren* map op de Docker Swarm-hoofdknooppunt, met behulp van de SSH-verbinding die u eerder hebt geconfigureerd. Zie het volgende scherm voor meer informatie.
    
    Bronmap: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Visual Studio teamservices - Release SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Configureren van een tweede taak voor het uitvoeren van een bash-opdracht uit te voeren `docker` en `docker stack deploy` opdrachten op het hoofdknooppunt. Zie het volgende scherm voor meer informatie.

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio teamservices - Release Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    De opdracht uitgevoerd op de hoofddoelserver gebruikt de Docker CLI en de CLI Docker-Compose naar de volgende taken uitvoeren:

    - Meld u aan bij Azure container registry (hierbij drie build-variabelen die zijn gedefinieerd in de **variabelen** tabblad)
    - Definieer de **DOCKER_HOST** variabele om te werken met het Swarm-eindpunt (: 2375)
    - Navigeer naar de *implementeren* map die is gemaakt met de vorige beveiligde kopie-taak en die de docker-compose.yml-bestand bevat 
    - Voer `docker stack deploy` opdrachten die de nieuwe afbeeldingen ophalen en de containers te maken.

    >[!IMPORTANT]
    > Zoals in het vorige scherm wordt weergegeven, laat de **mislukken op STDERR** selectievakje uitgeschakeld. Deze instelling kan we de release voltooien vanwege `docker-compose` worden afgedrukt verschillende diagnostische berichten, zoals containers zijn moet worden gestopt of wordt verwijderd, klikt u op de standaardfout-uitvoer. Als u het selectievakje Visual Studio Team Services-rapporten dat fouten zijn opgetreden tijdens de release, zelfs als het goed.
    >
3. Sla deze nieuwe release-definitie.

## <a name="step-4-test-the-cicd-pipeline"></a>Stap 4: De CI/CD-pijplijn testen

Nu u klaar bent met de configuratie, is het tijd om te testen van deze nieuwe CI/CD-pijplijn. De eenvoudigste manier om dit te testen, is voor het bijwerken van de broncode en de wijzigingen aan in uw GitHub-opslagplaats. Een paar seconden nadat u de code hebt gepusht, ziet u een nieuwe build in Visual Studio Team Services wordt uitgevoerd. Eenmaal is voltooid, wordt een nieuwe versie wordt geactiveerd en de nieuwe versie van de toepassing op het Azure Container Service-cluster geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over CI/CD met Visual Studio Team Services, de [VSTS Build-overzicht](https://www.visualstudio.com/docs/build/overview).
* Zie voor meer informatie over de ACS-Engine, het [ACS Engine GitHub-opslagplaats](https://github.com/Azure/acs-engine).
* Zie voor meer informatie over Docker Swarm-modus, de [overzicht van Docker Swarm-modus](https://docs.docker.com/engine/swarm/).
