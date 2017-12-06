---
title: CI/CD met Azure Container Service-Engine en Swarm-modus
description: Azure Container Service-Engine met Docker Swarm-modus, een Azure Container register en Visual Studio Team Services gebruiken om te leveren continu een toepassing met meerdere .NET Core
services: container-service
author: diegomrtnzg
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 6aa690ff7ec0689db78ff1225d36171adb30ee2c
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a>Volledige CI/CD-pijplijn voor het implementeren van een toepassing met meerdere container in Azure Container Service met de ACS-Engine en Docker Swarm-modus met behulp van Visual Studio Team Services

*In dit artikel is gebaseerd op [volledige CI/CD-pijplijn voor het implementeren van een toepassing met meerdere container in Azure Container Service met Docker Swarm met behulp van Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentatie*

Een van de grootste uitdagingen bij het ontwikkelen van moderne toepassingen voor de cloud wordt tegenwoordig deze toepassingen continu leveren. In dit artikel leert u hoe voor het implementeren van een volledige continue integratie en implementatie (CI/CD) pijplijn: 
* Azure Container Service-Engine met Docker Swarm-modus
* Azure Container Registry
* Visual Studio Team Services

In dit artikel is gebaseerd op een eenvoudige toepassing beschikbaar is op [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), ontwikkelde met ASP.NET Core. De toepassing bestaat uit vier verschillende services: drie web-API's en een web-front-end:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

Het doel is deze toepassing continu in een modus voor Docker Swarm-cluster met behulp van Visual Studio Team Services leveren. De volgende afbeelding om deze pipeline continue levering details:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Hier volgt een korte uitleg van de stappen uit:

1. Codewijzigingen zijn doorgevoerd in de opslagplaats van bron code (in dit geval GitHub) 
2. GitHub activeert een bouwen in Visual Studio Team Services 
3. Visual Studio Team Services haalt de meest recente versie van de bronnen en maakt de installatiekopieën waaruit de toepassing 
4. Visual Studio Team Services verstuurd elke installatiekopie naar een Docker-register gemaakt met behulp van de Azure-Container Registry-service 
5. Visual Studio Team Services activeert een nieuwe release 
6. De release van bepaalde opdrachten met SSH op het hoofdknooppunt van Azure container service-cluster wordt uitgevoerd 
7. Docker Swarm modus op het cluster haalt de meest recente versie van de installatiekopieën 
8. De nieuwe versie van de toepassing wordt geïmplementeerd met behulp van Docker-Stack 

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u de volgende taken uitvoeren:

- [Een modus Swarm-cluster maken in Azure Container Service met de ACS-Engine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Verbinding maken met het Swarm-cluster in Azure Container Service](../container-service-connect.md)
- [Een Azure container registry maken](../../container-registry/container-registry-get-started-portal.md)
- [Een Visual Studio Team Services-account en team project zijn gemaakt](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Vertakken de GitHub-opslagplaats met uw GitHub-account](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> De Docker Swarm-orchestrator in Azure Container Service maakt gebruik van legacy standalone Swarm. Momenteel is de geïntegreerde [Swarm-modus](https://docs.docker.com/engine/swarm/) (in Docker 1.12 en hoger) geen ondersteunde orchestrator in Azure Container Service. Daarom gebruiken we [ACS-Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), community-bijgedragen [snelstartsjabloon](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), of een Docker-oplossing in de [Azure Marketplace](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Stap 1: Uw Visual Studio Team Services-account configureren 

In deze sectie configureert u uw Visual Studio Team Services-account. VSTS-Services-eindpunten configureren in uw project voor Visual Studio Team Services, klikt u op de **instellingen** pictogram in de werkbalk en selecteert u **Services**.

![Open Services-eindpunt](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a>Verbinding maken met Visual Studio Team Services en Azure-account

Een verbinding tussen uw project VSTS en uw Azure-account instellen.

1. Klik aan de linkerkant op **nieuwe Service-eindpunt** > **Azure Resource Manager**.
2. Voor het autoriseren van VSTS werken met uw Azure-account, selecteer uw **abonnement** en klik op **OK**.

    ![Visual Studio teamservices - autoriseren van Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a>Verbinding maken met Visual Studio teamservices en GitHub

Een verbinding tussen uw project VSTS en uw GitHub-account instellen.

1. Klik aan de linkerkant op **nieuwe Service-eindpunt** > **GitHub**.
2. Als u wilt machtigen VSTS werken met uw GitHub-account, klikt u op **autoriseren** en volg de procedure in het venster dat wordt geopend.

    ![Visual Studio teamservices - autoriseren GitHub](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-to-your-azure-container-service-cluster"></a>VSTS verbinding met uw Azure Container Service-cluster

De laatste stappen voordat u in de pipeline CI/CD zijn voor het configureren van externe verbindingen met uw Docker Swarm-cluster in Azure. 

1. Voeg een eindpunt van het type voor de Docker Swarm-cluster **SSH**. Voer vervolgens de SSH-verbindingsgegevens van de Swarm-cluster (master knooppunt).

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Alle van de configuratie wordt nu uitgevoerd. In de volgende stappen maakt u de CI/CD-pijplijn die u maakt en implementeert de toepassing op het Docker Swarm-cluster. 

## <a name="step-2-create-the-build-definition"></a>Stap 2: De definitie van de build maken

In deze stap maakt u een build-definitie voor uw project VSTS instellen en definiëren van de build-werkstroom voor de installatiekopieën van de container

### <a name="initial-definition-setup"></a>Initiële definitie setup

1. De definitie van een build maken, koppelen aan uw project voor Visual Studio Team Services en klik op **bouwen & Release**. In de **bouwen definities** sectie, klikt u op **+ nieuw**. 

    ![Visual Studio teamservices - nieuwe bouwen definitie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Selecteer de **leeg proces**.

    ![Visual Studio teamservices - nieuwe, lege bouwen definitie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Klik vervolgens op de **variabelen** tabblad en twee nieuwe variabelen maken: **RegistryURL** en **AgentURL**. Plak de waarden van het register en DNS-Cluster-Agents.

    ![Visual Studio teamservices - variabelen Buildconfiguratie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. Op de **bouwen definities** pagina, open de **Triggers** tabblad en configureren van de build voor het gebruik van doorlopende integratie met de fork van het MyShop-project dat u hebt gemaakt in de vereisten. Selecteer **Batch wijzigingen**. Zorg ervoor dat u selecteert *docker-linux* als de **vertakking specificatie**.

    ![Visual Studio teamservices - configuratie van de Build-opslagplaats](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Ten slotte op de **opties** tabblad en configureren van de wachtrij agent naar **gehost Linux Preview**.

    ![Visual Studio teamservices - Agent de hostconfiguratie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a>Definieer de build-werkstroom
De volgende stappen definiëren de build-werkstroom. Eerst moet u de bron van de code te configureren. Selecteer hiervoor **GitHub** en uw **opslagplaats** en **vertakking** (docker-linux).

![Visual Studio teamservices - configureren broncode](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Er zijn vijf container afbeeldingen maken voor de *MyShop* toepassing. Elke installatiekopie gebouwd met behulp van de Dockerfile zich in de projectmappen:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* Winkelpagina

U moet twee Docker-stappen voor elke installatiekopie, één voor het bouwen van de installatiekopie en één voor de push-installatiekopie van het in het register van de Azure-container. 

1. Klik op als u een stap in de werkstroom build **+ toevoegen build stap** en selecteer **Docker**.

    ![Visual Studio teamservices - toevoegen Build stappen](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Voor elke installatiekopie, configureert u één stap die gebruikmaakt van de `docker build` opdracht.

    ![Visual Studio teamservices - Docker bouwen](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Voor de build-bewerking, selecteert u het register van de Container Azure de **bouwen van een installatiekopie van een** actie en de Dockerfile waarin elke installatiekopie. Instellen de **werkmap** als de hoofdmap Dockerfile definieert de **Installatiekopienaam**, en selecteer **meest recente code bevatten**.
    
    Naam van de installatiekopie moet worden in deze indeling: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Vervang **[NAME]** met de naam van de installatiekopie:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Voor elke installatiekopie configureert u een tweede stap die gebruikmaakt van de `docker push` opdracht.

    ![Visual Studio teamservices - Docker-Push](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Voor de push-bewerking, selecteert u het register Azure container de **een installatiekopie van een Push** actie, voer de **installatiekopie met de naam** die is ingebouwd in de vorige stap en selecteer **meest recente code bevatten**.

4. Nadat u de stappen build en pushmeldingen voor elk van de vijf afbeeldingen hebt geconfigureerd, moet u drie meer stappen toevoegen in de build-werkstroom.

   ![Visual Studio teamservices - toevoegen opdrachtregelprogramma taak](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. Een opdrachtregeltaak die gebruikmaakt van een bash-scripts ter vervanging van de *RegistryURL* exemplaar in de docker-compose.yml-bestand met de variabele RegistryURL. 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio teamservices - Update opstellen-bestand met de Register-URL](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. Een opdrachtregeltaak die gebruikmaakt van een bash-scripts ter vervanging van de *AgentURL* exemplaar in de docker-compose.yml-bestand met de variabele AgentURL.
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. Een taak die het bijgewerkte opstellen-bestand als een artefact build zakt zodat deze kan worden gebruikt in de release. Zie het volgende scherm voor meer informatie.

         ![Visual Studio teamservices - publiceren artefacten](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - opstellen publiceren bestand](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Klik op **opslaan en wachtrij** voor het testen van de definitie van de build.

   ![Visual Studio teamservices - opslaan en wachtrij](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio teamservices - nieuwe wachtrij](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Als de **bouwen** juist is, u moet dit scherm wordt weergegeven:

  ![Visual Studio teamservices - Build is voltooid](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-definition"></a>Stap 3: De release-definitie maken

Visual Studio Team Services kunt u [releases beheren in omgevingen](https://www.visualstudio.com/team-services/release-management/). U kunt continue implementatie om ervoor te zorgen dat uw toepassing wordt geïmplementeerd op de verschillende omgevingen (zoals ontwikkelen, testen, preproductie en productie) in een goede manier inschakelen. U kunt een omgeving met uw Azure Container Service Docker Swarm modus cluster maken.

![Visual Studio teamservices - ACS-versie](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Installatie van de eerste release

1. Klik op om de definitie van een release **Releases** > **+ Release**

2. Als u wilt de artefacten configureren, klikt u op **artefacten** > **een artefact koppelingsbron**. Hier definitie voor dit nieuwe release koppeling naar de versie die u hebt gedefinieerd in de vorige stap. Hierna is vindt de docker-compose.yml-bestand u in de release-proces.

    ![Visual Studio teamservices - Release artefacten](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. Voor het configureren van de trigger release, klikt u op **Triggers** en selecteer **continue implementatie**. De trigger niet instellen op dezelfde bron artefacten. Deze instelling zorgt ervoor dat er een nieuwe release wordt gestart wanneer de build voltooid is.

    ![Visual Studio teamservices - Release-Triggers](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Voor het configureren van de release-variabelen, klikt u op **variabelen** en selecteer **+ variabele** drie nieuwe variabelen maken met de gegevens van het register: **docker.username**, **docker.password**, en **docker.registry**. Plak de waarden van het register en DNS-Cluster-Agents.

    ![Visual Studio teamservices - configuratie van de Build-opslagplaats](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Zoals u in het vorige scherm, klikt u op de **vergrendeling** checkbox in docker.password. Deze instelling is belangrijk om te beperken van het wachtwoord.
    >

### <a name="define-the-release-workflow"></a>Definieer de release-werkstroom

De werkstroom release bestaat uit twee taken die u toevoegt.

1. Configureren van een taak voor het veilig de opstellen bestand kopiëren naar een *implementeren* map op het Docker Swarm hoofdknooppunt, met behulp van de SSH-verbinding die u eerder hebt geconfigureerd. Zie het volgende scherm voor meer informatie.
    
    Bronmap:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Visual Studio teamservices - Release SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Configureren van een tweede taak voor het uitvoeren van een bash-opdracht om uit te voeren `docker` en `docker stack deploy` opdrachten op het hoofdknooppunt. Zie het volgende scherm voor meer informatie.

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio teamservices - Release Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    De opdracht uitgevoerd op de hoofddoelserver gebruikt de Docker CLI en de CLI Docker Compose naar de volgende taken uitvoeren:

    - Aanmelden bij het register van de Azure-container (hierbij drie build-variabelen die zijn gedefinieerd in de **variabelen** tabblad)
    - Definieer de **DOCKER_HOST** variabele werken met het Swarm-eindpunt (: 2375)
    - Navigeer naar de *implementeren* map die door de voorgaande beveiligde kopie-taak is gemaakt en die de docker-compose.yml-bestand bevat 
    - Uitvoeren van `docker stack deploy` opdrachten die pull-de nieuwe afbeeldingen en de containers te maken.

    >[!IMPORTANT]
    > Zoals u in het vorige scherm, laat de **mislukken op STDERR** selectievakje uitgeschakeld. Deze instelling kunt u ons voltooien release vanwege `docker-compose` worden afgedrukt verschillende diagnostische berichten, zoals de containers zijn gestopt of op de uitvoer van de standaardfout wordt verwijderd. Als u het selectievakje inschakelt, rapporteert Visual Studio Team Services dat fouten zijn opgetreden tijdens de release, zelfs als alles goed gaat.
    >
3. Sla deze definitie voor de nieuwe versie.

## <a name="step-4-test-the-cicd-pipeline"></a>Stap 4: De pijplijn CI/CD testen

Nu u klaar bent met de configuratie, is het tijd voor het testen van dit nieuwe CI/CD-pijplijn. De eenvoudigste manier om deze te testen, is voor het bijwerken van de broncode en voer de wijzigingen in uw GitHub-opslagplaats. Enkele seconden nadat u de code pushen ziet u een nieuwe build uitgevoerd in Visual Studio Team Services. Zodra de installatie is voltooid, wordt een nieuwe release wordt geactiveerd en de nieuwe versie van de toepassing op het Azure Container Service-cluster wordt geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over CI/CD met Visual Studio Team Services, de [VSTS bouwen overzicht](https://www.visualstudio.com/docs/build/overview).
* Zie voor meer informatie over de ACS-Engine de [ACS-Engine GitHub-repo-](https://github.com/Azure/acs-engine).
* Zie voor meer informatie over Docker Swarm-modus, de [Docker Swarm modus overzicht](https://docs.docker.com/engine/swarm/).
