---
title: Doorlopend bouwen en integreren voor uw Azure Service Fabric Linux Java-toepassing met behulp van Jenkins | Microsoft Docs
description: Doorlopend bouwen en integreren voor uw Linux Java-toepassing met behulp van Jenkins
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 71e3d130f22515d22dc7f486f3dede936b874049
ms.lasthandoff: 03/25/2017


---
# <a name="use-jenkins-to-build-and-deploy-your-linux-java-application"></a>Jenkins gebruiken om uw Java-toepassing in Linux te bouwen en te implementeren
Jenkins is een populair hulpprogramma voor doorlopende integratie en implementatie van uw apps. Hier leest u hoe u een Azure Service Fabric-toepassing maakt en implementeert met behulp van Jenkins.

## <a name="general-prerequisites"></a>Algemene vereisten
- Zorg ervoor dat Git lokaal is geïnstalleerd. U kunt de juiste Git-versie (deze is afhankelijk van uw besturingssysteem) installeren via [de Git-downloadpagina](https://git-scm.com/downloads). Als u niet bekend bent met Git, raadpleegt u de [Git-documentatie](https://git-scm.com/docs) voor meer informatie.
- Zorg ervoor dat u de Jenkins-invoegtoepassing voor Service Fabric bij de hand hebt. U kunt deze ook downloaden via [Service Fabric-downloads](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins instellen in een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. In de volgende secties ziet u hoe u Jenkins instelt in een cluster.

### <a name="prerequisites"></a>Vereisten
1. Zorg ervoor dat u beschikt over een Service Fabric-cluster voor Linux. Voor een Service Fabric-cluster dat is gemaakt via Azure Portal, is Docker al geïnstalleerd. Als u het cluster lokaal uitvoert, controleert u met behulp van de opdracht ``docker info`` of Docker is geïnstalleerd. Als het niet is geïnstalleerd, installeert u het dienovereenkomstig met de volgende opdrachten:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Zorg er aan de hand van de volgende stappen voor dat de Service Fabric-containertoepassing in het cluster wordt geïmplementeerd:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker
cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080   # Azure CLI cluster connect command
bash Scripts/install.sh
```
Hiermee wordt een Jenkins-container in het cluster geïnstalleerd. Dit kan worden bewaakt met de Service Fabric Explorer.

### <a name="steps"></a>Stappen
1. Ga in uw browser naar ``http://PublicIPorFQDN:8081``. Hier vindt u het pad naar het eerste beheerderswachtwoord dat nodig is om u aan te melden. U kunt Jenkins blijven gebruiken als beheerder. Maar u kunt er ook voor kiezen om een gebruiker te maken en van account te wisselen nadat u zich met het oorspronkelijke beheerdersaccount hebt aangemeld.

   > [!NOTE]
   > Controleer tijdens het maken van het cluster of poort 8081 is opgegeven als de poort voor het toepassingseindpunt.
   >

2. Haal de exemplaar-id van de container op met ``docker ps -a``.
3. Meld u met Secure Shell (SSH) aan bij de container en plak het pad dat in de Jenkins-portal wordt weergegeven. Als in de portal bijvoorbeeld het pad `PATH_TO_INITIAL_ADMIN_PASSWORD` wordt getoond, voert u de volgende opdracht uit:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Stel GitHub in voor Jenkins door de stappen uit te voeren uit [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Een nieuwe SSH-sleutel genereren en toevoegen aan de SSH-agent).
    * Volg de instructies van GitHub om de SSH-sleutel te genereren en de SSH-sleutel toe te voegen aan het GitHub-account waarop de opslagplaats wordt gehost.
    * Voer de opdrachten die in de vorige koppeling zijn genoemd, uit in de Jenkins Docker-shell (en niet op uw host).
    * Gebruik de volgende opdracht om u vanaf uw host aan te melden bij de Jenkins-shell:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins instellen buiten een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. In de volgende secties ziet u hoe u Jenkins instelt buiten een cluster.

### <a name="prerequisites"></a>Vereisten
U moet Docker hebben geïnstalleerd. De volgende opdrachten kunnen worden gebruikt om Docker vanuit de terminal te installeren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Wanneer u ``docker info`` nu uitvoert in de terminal, ziet u in de uitvoer dat de Docker-service wordt uitgevoerd.

### <a name="steps"></a>Stappen
  1. Haal de Service Fabric-installatiekopie van de container voor Jenkins op: ``docker pull raunakpandya/jenkins:v1``
  2. Voer de installatiekopie van de container uit: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Haal de id op van het exemplaar van de installatiekopie van de container. U kunt een lijst van alle Docker-containers bekijken met de opdracht ``docker ps –a``
  4. Voer de volgende stappen uit om u aan te melden bij de Jenkins-portal:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Als de container-id 2d24a73b5964 is, gebruikt u 2d24.
    * Dit wachtwoord is vereist om u via de portal (``http://<HOST-IP>:8080``) aan te melden bij het Jenkins-dashboard
    * Nadat u zich voor het eerst hebt aangemeld, kunt u een eigen gebruikersaccount maken voor toekomstige doeleinden. U kunt echter ook doorgaan met het beheerdersaccount. Nadat u een gebruiker hebt gemaakt, moet u daarmee doorgaan.
  5. Stel GitHub in voor Jenkins door de stappen uit te voeren uit [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Een nieuwe SSH-sleutel genereren en toevoegen aan de SSH-agent).
        * Volg de instructies van GitHub om de SSH-sleutel te genereren en deze toe te voegen aan het GitHub-account waarop de opslagplaats wordt gehost.
        * Voer de opdrachten die in de vorige koppeling zijn genoemd, uit in de Jenkins Docker-shell (en niet op uw host).
        * Gebruik de volgende opdrachten om u vanaf uw host aan te melden bij de Jenkins-shell:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Zorg ervoor dat het cluster waarin of de computer waarop de installatiekopie van de Jenkins-container wordt gehost, een openbaar IP-adres heeft. Alleen zo kan het Jenkins-exemplaar meldingen ontvangen van GitHub.

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a>De Service Fabric-invoegtoepassing voor Jenkins installeren via de portal

1. Ga naar ``http://PublicIPorFQDN:8081``
2. Selecteer in het Jenkins-dashboard **Jenkins beheren** > **Plug-ins beheren** > **Geavanceerd**.
Hier kunt u een invoegtoepassing uploaden. Selecteer **Choose file** en selecteer daarna het bestand **serviceFabric.hpi** dat u bij de stap Vereisten hebt gedownload. Als u **Upload** selecteert, installeert Jenkins de invoegtoepassing automatisch. Sta opnieuw starten toe als hierom wordt gevraagd.

## <a name="create-and-configure-a-jenkins-job"></a>Een Jenkins-taak maken en configureren

1. Maak een **nieuw item** vanaf het dashboard.
2. Voer een itemnaam in (bijvoorbeeld **MyJob**). Selecteer **free-style project** en klik op **OK**.
3. Ga naar de pagina van de taak en klik op **Configure**.

   a. Geef in de sectie General onder **GitHub project** de URL op van uw GitHub-project. Dit is de URL waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins (Continue integratie, Continue implementatie), bijvoorbeeld ``https://github.com/sayantancs/SFJenkins``.

   b. Selecteer onder de sectie **Broncodebeheer** de optie **Git**. Geef de opslagplaats-URL op waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins, bijvoorbeeld ``https://github.com/sayantancs/SFJenkins.git``. U kunt hier ook opgeven welke vertakking u wilt maken, bijvoorbeeld **/master**.
4. Configureer uw *GitHub* (waar de opslagplaats wordt gehost), zodat deze kan communiceren met Jenkins. Voer de volgende stappen uit:

   a. Ga naar de pagina van de GitHub-opslagplaats. Ga naar **Instellingen** > **Integraties en services**.

   b. Selecteer **Add Service**, typ **Jenkins** en selecteer de **Jenkins-GitHub-invoegtoepassing**.

   c. Geef de Jenkins-webhook-URL op (standaard is dit ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klik op **add/update service**.

   d. Er wordt een testgebeurtenis verzonden naar uw Jenkins-exemplaar. Er komt een groen vinkje bij de webhook te staan in GitHub en uw project wordt gemaakt.

   e. Selecteer in de sectie **Build Triggers** de gewenste buildoptie. Voor dit voorbeeld wilt u een build activeren telkens wanneer er een push naar de opslagplaats plaatsvindt. Selecteer daarom de optie **GitHub hook trigger for GITScm polling**. (Voorheen heette deze optie **Build when a change is pushed to GitHub**.)

   f. Ga naar de sectie **Build** en selecteer in de vervolgkeuzelijst **Add build step** de optie **Invoke Gradle Script**. Geef in de volgende widget het pad op naar het **hoofdscript** van uw toepassing. build.gradle wordt opgehaald uit het opgegeven pad en werkt dienovereenkomstig. Als u een project maakt met de naam ``MyActor`` (met behulp van de Eclipse-invoegtoepassing of Yeoman-generator), moet het hoofdscript ``${WORKSPACE}/MyActor`` bevatten. Bekijk de volgende schermafbeelding voor een voorbeeld van hoe dit er uitziet:

    ![Service Fabric-bouwactie voor Jenkins][build-step]

   g. Selecteer in de vervolgkeuzelijst **Post-Build Actions** de optie **Post-Build Actions**. Hier moet u clustergegevens opgeven, zoals waar de via Jenkins gecompileerde Service Fabric-toepassing wordt geïmplementeerd. U kunt ook aanvullende toepassingsgegevens opgeven. Deze worden gebruikt om de toepassing te implementeren. Bekijk de volgende schermafbeelding voor een voorbeeld van hoe dit er uitziet:

    ![Service Fabric-bouwactie voor Jenkins][post-build-step]

   > [!NOTE]
   > Het cluster dat u hier gebruikt, moet het cluster zijn waarin de Jenkins-containertoepassing wordt gehost als u Service Fabric gebruikt om de installatiekopie van de Jenkins-containerinstallatiekopie te implementeren.
   >

## <a name="next-steps"></a>Volgende stappen
GitHub en Jenkins zijn nu geconfigureerd. U zou een aantal voorbeeldwijzigingen in uw ``MyActor``-project in de voorbeeldopslagplaats https://github.com/sayantancs/SFJenkins kunnen aanbrengen. Push de wijzigingen naar een externe ``master``-vertakking (of een andere vertakking die u hebt geconfigureerd om mee te werken). Op die manier wordt de geconfigureerde Jenkins-taak ``MyJob`` geactiveerd. Hiermee worden de wijzigingen opgehaald van GitHub. Daarna worden ze gebouwd en wordt de toepassing geïmplementeerd in het clustereindpunt dat u hebt opgegeven voor de acties ná het bouwen.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

