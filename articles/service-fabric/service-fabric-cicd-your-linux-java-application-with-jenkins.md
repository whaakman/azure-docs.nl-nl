---
title: Doorlopend bouwen en implementeren voor uw Linux Java-toepassing met behulp van Jenkins | Microsoft Docs
description: Doorlopend bouwen en implementeren voor uw Linux Java-toepassing met behulp van Jenkins
services: service-fabric
documentationcenter: java
author: sayantancs
manager: raunakp
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: sayantancs
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: d3c64e3ed78a5379592b5e4276c83d26f04d2e1c
ms.lasthandoff: 03/08/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Uw Linux Java-toepassing bouwen en implementeren met behulp van Jenkins
Jenkins is een populair hulpprogramma voor doorlopende integratie en implementatie. In dit document leest u alles over het bouwen en implementeren van uw Service Fabric-toepassing met behulp van Jenkins.

## <a name="general-prerequisites"></a>Algemene vereisten
1. U moet Git lokaal hebben geïnstalleerd. U kunt de juiste Git-versie voor uw besturingssysteem [hier](https://git-scm.com/downloads) downloaden. Als u niet bekend bent met Git, volgt u de stappen in de [documentatie](https://git-scm.com/docs) om er kennis over op te doen.
2. U moet de Jenkins-invoegtoepassing voor Service Fabric bij de hand hebben. Download de Jenkins-invoegtoepassing voor Service Fabric [hier](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins instellen in een Service Fabric-cluster

### <a name="prerequisites"></a>Vereisten
1. Zorg ervoor dat u beschikt over een Service Fabric-cluster voor Linux. Voor Service Fabric-clusters die zijn gemaakt via Azure Portal, is Docker al geïnstalleerd. Als u het lokale cluster uitvoert, controleert u of Docker is geïnstalleerd of niet. Dat doet u met de opdracht ``docker info``. Als Docker niet is geïnstalleerd, voert u de installatie uit met -
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
2. Zorg ervoor dat de Service Fabric-containertoepassing wordt geïmplementeerd in het cluster ``http://PublicIPorFQDN:19080``. Volg de onderstaande stappen -
```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker && cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080 # Azure CLI cluster connect command
bash Scripts/install.sh
```
  Hiermee installeert u de Jenkins-container in het cluster waar u verbinding mee hebt gemaakt. U kunt de containertoepassing bewaken met Service Fabric Explorer.

### <a name="steps"></a>Stappen
1. Ga in de browser naar de URL ``http://PublicIPorFQDN:8081``. Hier vindt u het pad naar het eerste beheerderswachtwoord dat nodig is voor het aanmelden. U kunt Jenkins blijven gebruiken als beheerder, maar u kunt ook een gebruiker maken en wijzigen nadat u zich hebt aangemeld bij het eerste beheerdersaccount.
> [!NOTE]
> U moet er tijdens het maken van het cluster voor zorgen dat poort 8081 is opgegeven als eindpuntpoort voor de toepassing
>

2. U kunt de containerexemplaar-id ophalen met ``docker ps -a``.
3. Meld u op basis van SSH aan bij de container en plak het pad dat aan u is getoond, in de Jenkins-portal. Als in de portal het pad `PATH_TO_INITIAL_ADMIN_PASSWORD` wordt getoond, kunt u bijvoorbeeld -  
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash # This takes you inside docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Stel GitHub [hiermee](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) in voor gebruik met Jenkins.
    * Volg de instructies van GitHub om de SSH-sleutel te maken en voeg de SSH-sleutel toe aan het GitHub-account waarop de opslagplaats wordt (of zou worden) gehost.
    * Voer de opdrachten die via de bovenstaande koppeling te zien zijn, uit in de Jenkins Docker-shell (en niet op uw host)
    * Als u zich vanaf uw host wilt aanmelden bij de Jenkins-shell, moet u -
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins instellen buiten een Service Fabric-cluster

### <a name="prerequisites"></a>Vereisten
1. U moet Docker hebben geïnstalleerd. Als dat niet het geval is, voert u de volgende opdrachten in de terminal in om Docker te installeren.
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
Wanneer u nu ``docker info`` uitvoert in de terminal, ziet u in de uitvoer dat de Docker-service wordt uitgevoerd.

### <a name="steps"></a>Stappen
  1. Haal de Service Fabric-container voor Jenkins op: ``docker pull IMAGE_NAME ``
  2. Voer de installatiekopie van de container uit: ``docker run -itd -p 8080:8080 IMAGE_NAME``
  3. Haal de id van de Docker-container op waarin de Jenkins-installatiekopie aanwezig is (die u zojuist hebt geïnstalleerd). U kunt een lijst van alle Docker-containers bekijken met de opdracht ``docker ps –a``
  4. Haal het beheerderswachtwoord op voor het Jenkins-account. Daarvoor doet u het volgende -
    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Als de container-id 2d24a73b5964 is, hoeft u alleen 2d24 in te voegen
    * Dit wachtwoord is vereist om u via de portal (``http://<HOST-IP>:8080``) aan te melden bij het Jenkins-dashboard
    * Wanneer u zich voor het eerst aanmeldt, kunt u een eigen gebruikersaccount maken voor toekomstige doeleinden. U kunt echter ook doorgaan met het beheerdersaccount. Als u een nieuwe gebruiker maakt, moet u daarmee doorgaan.
  5. Stel GitHub [hiermee](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) in voor gebruik met Jenkins.
      * Volg de instructies van GitHub om de SSH-sleutel te maken en voeg de SSH-sleutel toe aan het GitHub-account waarop de opslagplaats wordt (of zou worden) gehost.
      * Voer de opdrachten die via de bovenstaande koppeling te zien zijn, uit in de Jenkins Docker-shell (en niet op uw host)
      * Als u zich vanaf uw host wilt aanmelden bij de Jenkins-shell, moet u -
    ```sh
    docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
    ```

> [!NOTE]
> Zorg ervoor dat het cluster/de machine waarop de Jenkins-containerinstallatiekopie wordt gehost, beschikt over een openbaar IP-adres, zodat meldingen van GitHub worden ontvangen door het Jenkins-exemplaar.
>

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>De Service Fabric-invoegtoepassing voor Jenkins installeren via de portal

1. Ga naar ``http://PublicIPorFQDN:8081``
2. Selecteer ``Manage Jenkins`` -> ``Manage Plugins`` -> ``Advanced`` in het Jenkins-dashboard.
U kunt hier een invoegtoepassing uploaden. Selecteer de optie ``Choose file`` en selecteer daarna het bestand serviceFabric.hpi dat u bij de stap Vereisten hebt gedownload. Wanneer u Uploaden selecteert, installeert Jenkins automatisch de invoegtoepassing voor u. Mogelijk moet er opnieuw worden opgestart. Sta dit toe.

## <a name="creating-and-configuring-a-jenkins-job"></a>Een Jenkins-taak maken en configureren

1. Maak een ``new item`` via het dashboard
2. Voer een nieuwe itemnaam in, zoals ``MyJob``. Selecteer een freestyleproject en klik op OK
3. Ga naar de pagina van de taak en klik op ``Configure`` -
  * In de sectie Algemeen geeft u onder ``Github project`` de GitHub-project-URL op die wordt gebruikt voor het hosten van de Service Fabric Java-toepassing die u wilt integreren in de Jenkins CI/CD-werkstroom (bijvoorbeeld ``https://github.com/sayantancs/SFJenkins``).
  * Selecteer in de sectie ``Source Code Management`` de optie ``Git``. Geef de opslagplaats-URL op die wordt gebruikt voor het hosten van de Service Fabric Java-toepassing die u wilt integreren in de Jenkins CI/CD-werkstroom (bijvoorbeeld ``https://github.com/sayantancs/SFJenkins.git``). U kunt hier ook opgeven welke vertakking u wilt maken, zoals ``*/master``.
4. Configureer uw *GitHub* (waar de opslagplaats wordt gehost), zodat deze kan communiceren met Jenkins. Volg daarvoor deze stappen -
  - Ga naar de pagina van de GitHub-opslagplaats. Ga naar ``Settings`` -> ``Integrations and Services``.
  - Selecteer ``Add Service``, typ Jenkins en selecteer ``Jenkins-Github plugin``.
  - Geef de Jenkins-webhook-URL op (standaard is dit ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klik op Service toevoegen/bijwerken.
  - Er wordt een testgebeurtenis verzonden naar uw Jenkins-exemplaar. Er komt een groen vinkje bij de webhook te staan in GitHub en uw project wordt gemaakt!
  - In het gedeelte ``Build Triggers`` selecteert u welke bouwoptie u wilt gebruiken. In dit geval activeert u een bouwgebeurtenis wanneer er wordt gepusht naar de opslagplaats. De bijbehorende optie is dan dus ``GitHub hook trigger for GITScm polling``. (Voorheen heette dit 'Bouwen wanneer een wijziging naar GitHub wordt gepusht'.)
  - Bij ``Build section`` selecteert u in het vervolgkeuzemenu ``Add build step`` de optie ``Invoke Gradle Script``. In de widget die wordt geopend, geeft u voor uw toepassing het pad op naar ``Root build script``. build.gradle wordt opgehaald via het opgegeven pad. Als u een project maakt met de naam ``MyActor`` (met behulp van de invoegtoepassing Eclipse of de Yeoman-generator), moet het hoofdscript ``${WORKSPACE}/MyActor`` bevatten. Deze sectie ziet er (als voorbeeld) als volgt uit -

    ![Service Fabric-bouwactie voor Jenkins][build-step]
  - Selecteer de optie ``Deploy Service Fabric Project`` in het vervolgkeuzemenu ``Post-Build Actions``. U moet hier clustergegevens opgeven, zoals waar de via Jenkins opgestelde Service Fabric-toepassing wordt geïmplementeerd en andere toepassingsgegevens die nodig zijn voor het implementeren van de toepassing. De volgende schermafbeelding kan als uitgangspunt worden gebruikt:

    ![Service Fabric-bouwactie voor Jenkins][post-build-step]

 >[!Note]
 > Het cluster dat u hier gebruikt, moet het cluster zijn waarin de Jenkins-containertoepassing wordt gehost als u Service Fabric gebruikt om de Jenkins-containerinstallatiekopie te implementeren.
 >

### <a name="end-to-end-scenario"></a>End-to-end-scenario
GitHub en Jenkins zijn nu geconfigureerd. U kunt een voorbeeldwijziging aanbrengen aan uw project ``MyActor`` in de opslagplaats, zoals https://github.com/sayantancs/SFJenkins. Push uw wijzigingen daarna naar de externe ``master``-vertakking (of een vertakking die u zodanig hebt geconfigureerd dat u ermee kunt werken). Op die manier wordt de Jenkins-taak ``MyJob`` geactiveerd die u hebt geconfigureerd. Hiermee worden de wijzigingen opgehaald van GitHub. Daarna worden ze gebouwd en wordt de toepassing geïmplementeerd in het clustereindpunt dat u hebt opgegeven voor de acties ná het bouwen.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

