---
title: CI/CD met Azure Containerservice- en Swarm
description: Azure Container Service met Docker Swarm, een Azure Container register en Visual Studio Team Services gebruiken om te leveren continu een toepassing met meerdere .NET Core
services: container-service
author: jcorioland
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 38877afb63e993eeaab723a6ea5f4c40d3c956a5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Volledige CI/CD-pijplijn voor het implementeren van een toepassing met meerdere container in Azure Container Service met Docker Swarm met behulp van Visual Studio Team Services

Een van de grootste uitdagingen bij het ontwikkelen van moderne toepassingen voor de cloud wordt deze toepassingen continu leveren. In dit artikel leert u hoe u een volledige continue integratie en implementatie (CI/CD)-pijplijn met Azure Container Service met Docker Swarm, het register van Azure-Container en Visual Studio Team Services build te implementeren en releasebeheer.

In dit artikel is gebaseerd op een eenvoudige toepassing beschikbaar is op [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), ontwikkelde met ASP.NET Core. De toepassing bestaat uit vier verschillende services: drie web-API's en een web-front-end:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Het doel is deze toepassing continu in een Docker Swarm-cluster met behulp van Visual Studio Team Services leveren. De volgende afbeelding om deze pipeline continue levering details:

![De voorbeeldtoepassing MyShop](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Hier volgt een korte uitleg van de stappen uit:

1. Codewijzigingen zijn doorgevoerd in de opslagplaats van bron code (in dit geval GitHub) 
2. GitHub activeert een bouwen in Visual Studio Team Services 
3. Visual Studio Team Services haalt de meest recente versie van de bronnen en maakt de installatiekopieën waaruit de toepassing 
4. Visual Studio Team Services verstuurd elke installatiekopie naar een Docker-register gemaakt met behulp van de Azure-Container Registry-service 
5. Visual Studio Team Services activeert een nieuwe release 
6. De release van bepaalde opdrachten met SSH op het hoofdknooppunt van Azure container service-cluster wordt uitgevoerd 
7. Docker Swarm op het cluster haalt de meest recente versie van de installatiekopieën 
8. De nieuwe versie van de toepassing wordt geïmplementeerd met behulp van Docker Compose 

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u de volgende taken uitvoeren:

- [Een Swarm-cluster in Azure Container Service maken](container-service-deployment.md)
- [Verbinding maken met het Swarm-cluster in Azure Container Service](../container-service-connect.md)
- [Een Azure container registry maken](../../container-registry/container-registry-get-started-portal.md)
- [Een Visual Studio Team Services-account en team project zijn gemaakt](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Vertakken de GitHub-opslagplaats met uw GitHub-account](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

U moet ook een machine Ubuntu (14.04 of 16.04) met Docker geïnstalleerd. Deze computer wordt gebruikt door Visual Studio Team Services tijdens het bouwen en release. Een manier om deze machine te maken is het gebruik van de installatiekopie die beschikbaar zijn in de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Stap 1: Uw Visual Studio Team Services-account configureren 

In deze sectie configureert u uw Visual Studio Team Services-account.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Een Visual Studio Team Services Linux build-agent configureren

Als u wilt Docker-installatiekopieën maken en toepassen van deze installatiekopieën naar een Azure container register van de versie van een Visual Studio Team Services, moet u een Linux-agent registreren. U hebt deze installatieopties:

* [Een op Linux-agent implementeren](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Docker gebruiken voor het uitvoeren van de agent VSTS](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a>De uitbreiding voor de Docker-integratie VSTS installeren

Microsoft biedt een uitbreiding VSTS om te werken met Docker gebouwd en release van processen. Deze uitbreiding is beschikbaar in de [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Klik op **installeren** deze extensie toevoegen aan uw account VSTS:

![De Docker-integratie installeren](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

U wordt gevraagd of u verbinding maken met uw VSTS rekening met uw referenties. 

### <a name="connect-visual-studio-team-services-and-github"></a>Verbinding maken met Visual Studio teamservices en GitHub

Een verbinding tussen uw project VSTS en uw GitHub-account instellen.

1. In het project voor Visual Studio Team Services, klikt u op de **instellingen** pictogram in de werkbalk en selecteert u **Services**.

    ![Visual Studio teamservices - externe verbinding](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. Klik aan de linkerkant op **nieuwe Service-eindpunt** > **GitHub**.

    ![Visual Studio teamservices - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. Als u wilt machtigen VSTS werken met uw GitHub-account, klikt u op **autoriseren** en volg de procedure in het venster dat wordt geopend.

    ![Visual Studio teamservices - autoriseren GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a>VSTS verbinding met uw Azure-container register en Azure Container Service-cluster

De laatste stappen voordat u in de pipeline CI/CD zijn voor het configureren van externe verbindingen met het register van de container en Docker Swarm-cluster in Azure. 

1. In de **Services** instellingen van uw project Visual Studio Team Services, Voeg een service-eindpunt van het type **Docker-register**. 

2. Voer de URL en de referenties van de registratie van uw Azure-container in de pop-up dat wordt geopend.

    ![Visual Studio teamservices - Docker-register](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. Voeg een eindpunt van het type voor de Docker Swarm-cluster **SSH**. Voer vervolgens de SSH-verbindingsgegevens van de Swarm-cluster.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Alle van de configuratie wordt nu uitgevoerd. In de volgende stappen maakt u de CI/CD-pijplijn die u maakt en implementeert de toepassing op het Docker Swarm-cluster. 

## <a name="step-2-create-the-build-definition"></a>Stap 2: De definitie van de build maken

In deze stap maakt u een build definitionfor instellen met uw project VSTS en definiëren van de build-werkstroom voor de installatiekopieën van de container

### <a name="initial-definition-setup"></a>Initiële definitie setup

1. De definitie van een build maken, koppelen aan uw project voor Visual Studio Team Services en klik op **bouwen & Release**. 

2. In de **bouwen definities** sectie, klikt u op **+ nieuw**. Selecteer de **leeg** sjabloon.

    ![Visual Studio teamservices - nieuwe bouwen definitie](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. De nieuwe build configureren met een GitHub-opslagplaats bron, selectievakje **continue integratie**, en selecteert u de agent wachtrij waar u uw Linux-agent is geregistreerd. Klik op **maken** de build-definitie maken.

    ![Visual Studio teamservices - maken Build-definitie](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. Op de **bouwen definities** pagina, opent u eerst de **opslagplaats** tabblad en configureren van de build voor het gebruik van de fork van het MyShop-project dat u hebt gemaakt in de vereisten. Zorg ervoor dat u selecteert *acs docs* als de **standaardvertakking**.

    ![Visual Studio teamservices - configuratie van de Build-opslagplaats](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. Op de **Triggers** tabblad, het configureren van de build worden geactiveerd na elke doorvoer. Selecteer **continue integratie** en **Batch wijzigingen**.

    ![Visual Studio teamservices - configuratie van de Build-Trigger](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Definieer de build-werkstroom
De volgende stappen definiëren de build-werkstroom. Er zijn vijf container afbeeldingen maken voor de *MyShop* toepassing. Elke installatiekopie gebouwd met behulp van de Dockerfile zich in de projectmappen:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* Winkelpagina

U moet twee Docker-stappen voor elke installatiekopie, één voor het bouwen van de installatiekopie en één voor de push-installatiekopie van het in het register van de Azure-container toevoegen. 

1. Klik op als u een stap in de werkstroom build **+ toevoegen build stap** en selecteer **Docker**.

    ![Visual Studio teamservices - toevoegen Build stappen](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. Voor elke installatiekopie, configureert u één stap die gebruikmaakt van de `docker build` opdracht.

    ![Visual Studio teamservices - Docker bouwen](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Voor de build-bewerking, selecteert u het register Azure container de **bouwen van een installatiekopie van een** actie en de Dockerfile waarin elke installatiekopie. Stel de **Build context** als de Dockerfile hoofdmap en definieer de **installatiekopie met de naam**. 
    
    Zoals u in het vorige scherm, start u de naam van de installatiekopie met de URI van de registratie van uw Azure-container. (U kunt ook een build-variabele gebruiken om te voorzien van het label van de afbeelding, zoals de build-id in dit voorbeeld.)

3. Voor elke installatiekopie configureert u een tweede stap die gebruikmaakt van de `docker push` opdracht.

    ![Visual Studio teamservices - Docker-Push](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Voor de push-bewerking, selecteert u het register Azure container de **een installatiekopie van een Push** actie en voer de **Installatiekopienaam** die is ingebouwd in de vorige stap.

4. Nadat u de stappen build en pushmeldingen voor elk van de vijf afbeeldingen hebt geconfigureerd, moet u twee meer stappen toevoegen in de build-werkstroom.

    a. Een opdrachtregeltaak die gebruikmaakt van een bash-scripts ter vervanging van de *BuildNumber* exemplaar in de docker-compose.yml-bestand met de huidige id maken Zie het volgende scherm voor meer informatie.

    ![Visual Studio teamservices - Update opstellen bestand](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Een taak die het bijgewerkte opstellen-bestand als een artefact build zakt zodat deze kan worden gebruikt in de release. Zie het volgende scherm voor meer informatie.

    ![Visual Studio Team Services - opstellen publiceren bestand](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. Klik op **opslaan** en de naam van de definitie van de build.

## <a name="step-3-create-the-release-definition"></a>Stap 3: De release-definitie maken

Visual Studio Team Services kunt u [releases beheren in omgevingen](https://www.visualstudio.com/team-services/release-management/). U kunt continue implementatie om ervoor te zorgen dat uw toepassing wordt geïmplementeerd op de verschillende omgevingen (zoals ontwikkelen, testen, preproductie en productie) in een goede manier inschakelen. U kunt een nieuwe omgeving met uw Azure Container Service Docker Swarm-cluster maken.

![Visual Studio teamservices - ACS-versie](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Installatie van de eerste release

1. Klik op om de definitie van een release **Releases** > **+ Release**

2. Als u wilt de artefacten configureren, klikt u op **artefacten** > **een artefact koppelingsbron**. Hier definitie voor dit nieuwe release koppeling naar de versie die u hebt gedefinieerd in de vorige stap. Dit doet, is de docker-compose.yml-bestand beschikbaar in de release-proces.

    ![Visual Studio teamservices - Release artefacten](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. Voor het configureren van de trigger release, klikt u op **Triggers** en selecteer **continue implementatie**. De trigger niet instellen op dezelfde bron artefacten. Deze instelling zorgt ervoor dat er een nieuwe release begint zodra de build voltooid is.

    ![Visual Studio teamservices - Release-Triggers](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Definieer de release-werkstroom

De werkstroom release bestaat uit twee taken die u toevoegt.

1. Configureren van een taak voor het veilig de opstellen bestand kopiëren naar een *implementeren* map op het Docker Swarm hoofdknooppunt, met behulp van de SSH-verbinding die u eerder hebt geconfigureerd. Zie het volgende scherm voor meer informatie.

    ![Visual Studio teamservices - Release SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. Configureren van een tweede taak voor het uitvoeren van een bash-opdracht om uit te voeren `docker` en `docker-compose` opdrachten op het hoofdknooppunt. Zie het volgende scherm voor meer informatie.

    ![Visual Studio teamservices - Release Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    De opdracht wordt uitgevoerd op de master gebruikt de Docker CLI en Docker Compose CLI om de volgende taken uitvoeren:

    - Meld u aan bij het register van de Azure-container (hierbij drie build variab'les die zijn gedefinieerd in de **variabelen** tabblad)
    - Definieer de **DOCKER_HOST** variabele werken met het Swarm-eindpunt (: 2375)
    - Navigeer naar de *implementeren* map die door de voorgaande beveiligde kopie-taak is gemaakt en die de docker-compose.yml-bestand bevat 
    - Uitvoeren van `docker-compose` opdrachten die de nieuwe afbeeldingen pull stop de services, verwijder de services en de containers te maken.

    >[!IMPORTANT]
    > Zoals u in het vorige scherm, laat de **mislukken op STDERR** selectievakje uitgeschakeld. Dit is een belangrijk instelling, omdat `docker-compose` worden afgedrukt verschillende diagnostische berichten, zoals de containers zijn gestopt of op de uitvoer van de standaardfout wordt verwijderd. Als u het selectievakje inschakelt, rapporteert Visual Studio Team Services dat fouten zijn opgetreden tijdens de release, zelfs als alles goed gaat.
    >
3. Sla deze definitie voor de nieuwe versie.


>[!NOTE]
>Deze implementatie bevat enige uitvaltijd, omdat we stoppen van de oude services en waarop de nieuwe database wordt uitgevoerd. Het is mogelijk om te voorkomen dat dit als volgt een blauw-groen-implementatie.
>

## <a name="step-4-test-the-cicd-pipeline"></a>Stap 4. De pijplijn CI/CD testen

Nu u klaar bent met de configuratie, is het tijd voor het testen van dit nieuwe CI/CD-pijplijn. De eenvoudigste manier om deze te testen, is voor het bijwerken van de broncode en voer de wijzigingen in uw GitHub-opslagplaats. Enkele seconden nadat u de code pushen ziet u een nieuwe build uitgevoerd in Visual Studio Team Services. Zodra de installatie is voltooid, wordt een nieuwe release wordt geactiveerd en de nieuwe versie van de toepassing op het Azure Container Service-cluster implementeert.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over CI/CD met Visual Studio Team Services, de [VSTS bouwen overzicht](https://www.visualstudio.com/docs/build/overview).
