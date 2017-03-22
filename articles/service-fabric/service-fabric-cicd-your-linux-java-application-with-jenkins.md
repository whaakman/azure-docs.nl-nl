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
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 335f41e2e4a40e64e87382ea338673de3ab79c27
ms.lasthandoff: 03/14/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Uw Linux Java-toepassing bouwen en implementeren met behulp van Jenkins
Jenkins is een populair hulpprogramma voor doorlopende integratie en implementatie. In dit document leest u alles over het bouwen en implementeren van uw Service Fabric-toepassing met behulp van Jenkins.

## <a name="general-prerequisites"></a>Algemene vereisten
1. U moet Git lokaal hebben geïnstalleerd. U kunt de juiste Git-versie voor uw besturingssysteem [hier](https://git-scm.com/downloads) downloaden. Als u niet bekend bent met Git, volgt u de stappen in de [documentatie](https://git-scm.com/docs) om er kennis over op te doen.
2. U moet de Jenkins-invoegtoepassing voor Service Fabric bij de hand hebben. Download de Jenkins-invoegtoepassing voor Service Fabric [hier](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins instellen in een Service Fabric-cluster

### <a name="prerequisites"></a>Vereisten
1. Zorg ervoor dat u beschikt over een Service Fabric-cluster voor Linux. Voor een Service Fabric-cluster dat is gemaakt via Azure Portal, is Docker al geïnstalleerd. Als u het cluster lokaal uitvoert, controleert u of Docker is geïnstalleerd of niet. Dat doet u met de opdracht ``docker info``. Als Docker niet is geïnstalleerd, voert u de installatie uit met de volgende opdrachten:

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
Hiermee wordt de Jenkins-container in het cluster geïnstalleerd. Dit kan worden bewaakt met de Service Fabric Explorer.

### <a name="steps"></a>Stappen
1. Ga in de browser naar de URL ``http://PublicIPorFQDN:8081``. Hier vindt u het pad naar het eerste beheerderswachtwoord dat nodig is voor het aanmelden. U kunt Jenkins blijven gebruiken als beheerder, maar u kunt ook een gebruiker maken en wijzigen nadat u zich hebt aangemeld bij het eerste beheerdersaccount.

  > [!NOTE]
  > U moet er tijdens het maken van het cluster voor zorgen dat poort 8081 is opgegeven als eindpuntpoort voor de toepassing
  >

2. U kunt de containerexemplaar-id ophalen met ``docker ps -a``.
3. Meld u op basis van SSH aan bij de container en plak het pad dat aan u is getoond in de Jenkins-portal. Als in de portal het pad `PATH_TO_INITIAL_ADMIN_PASSWORD` wordt getoond, kunt u bijvoorbeeld -

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Stel GitHub in voor gebruik met Jenkins door de stappen te volgen die u ziet door de [koppeling](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) te volgen.
    * Volg de instructies van GitHub om de SSH-sleutel te maken en voeg de SSH-sleutel toe aan het GitHub-account waarop de opslagplaats wordt gehost.
    * Voer de opdrachten die via de vorige koppeling te zien zijn, uit in de Jenkins Docker-shell (en niet op uw host)
    * Als u zich vanaf uw host wilt aanmelden bij de Jenkins-shell, voert u de volgende opdracht uit:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins instellen buiten een Service Fabric-cluster

### <a name="prerequisites"></a>Vereisten
U moet Docker hebben geïnstalleerd. De volgende opdrachten kunnen worden gebruikt om Docker vanuit de terminal te installeren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Wanneer u nu ``docker info`` uitvoert in de terminal, ziet u in de uitvoer dat de Docker-service wordt uitgevoerd.

### <a name="steps"></a>Stappen
  1. Haal de Service Fabric-installatiekopie van de container voor Jenkins op: ``docker pull servicefabric-microsoft.azurecr.io/jenkins:v1``
  2. Voer de installatiekopie van de container uit: ``docker run -itd -p 8080:8080 servicefabric-microsoft.azurecr.io/jenkins:v1``
  3. Haal de id van het exemplaar van de installatiekopie van de container op. U kunt een lijst van alle Docker-containers bekijken met de opdracht ``docker ps –a``
  4. Meld u aan de hand van de volgende stappen aan bij de Jenkins-portal:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Als de container-id 2d24a73b5964 is, gebruikt u 2d24.
    * Dit wachtwoord is vereist om u via de portal (``http://<HOST-IP>:8080``) aan te melden bij het Jenkins-dashboard
    * Wanneer u zich voor het eerst aanmeldt, kunt u een eigen gebruikersaccount maken voor toekomstige doeleinden. U kunt echter ook doorgaan met het beheerdersaccount. Als u een gebruiker maakt, moet u daarmee doorgaan.
  5. Stel GitHub in voor gebruik met Jenkins door de stappen te volgen die u ziet door de [koppeling](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) te volgen.
      * Volg de instructies van GitHub om de SSH-sleutel te maken en voeg de SSH-sleutel toe aan het GitHub-account waarop de opslagplaats wordt (of zou worden) gehost.
      * Voer de opdrachten die via de vorige koppeling te zien zijn, uit in de Jenkins Docker-shell (en niet op uw host)
      * Als u zich vanaf uw host wilt aanmelden bij de Jenkins-shell, voert u de volgende opdrachten uit:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

    > [!NOTE]
    > Zorg ervoor dat het cluster/de machine waarop de Jenkins-containerinstallatiekopie wordt gehost, beschikt over een openbaar IP-adres, zodat meldingen van GitHub worden ontvangen door het Jenkins-exemplaar.
    >

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>De Service Fabric-invoegtoepassing voor Jenkins installeren via de portal

1. Ga naar ``http://PublicIPorFQDN:8081``
2. Selecteer in het Jenkins-dashboard **Jenkins beheren** -> **Plug-ins beheren** -> **Geavanceerd**.
U kunt hier een invoegtoepassing uploaden. Selecteer de optie **Bestand kiezen** en selecteer daarna het bestand serviceFabric.hpi dat u bij de stap Vereisten hebt gedownload. Wanneer u Uploaden selecteert, installeert Jenkins de invoegtoepassing automatisch. Sta opnieuw starten toe als hierom wordt gevraagd.

## <a name="creating-and-configuring-a-jenkins-job"></a>Een Jenkins-taak maken en configureren

1. Maak een **nieuw item** vanaf het dashboard
2. Voer een itemnaam in, zoals **MyJob**. Selecteer een freestyleproject en klik op OK
3. Ga naar de pagina van de taak en klik op **Configureren** -
  * In de sectie Algemeen geeft u onder **GitHub-project** de GitHub-project-URL op die wordt gebruikt voor het hosten van de Service Fabric Java-toepassing die u wilt integreren in de Jenkins CI/CD-werkstroom (bijvoorbeeld ``https://github.com/sayantancs/SFJenkins``).
  * Selecteer onder de sectie **Broncodebeheer** de optie **Git**. Geef de opslagplaats-URL op die wordt gebruikt voor het hosten van de Service Fabric Java-toepassing die u wilt integreren in de Jenkins CI/CD-werkstroom (bijvoorbeeld ``https://github.com/sayantancs/SFJenkins.git``). U kunt hier ook opgeven welke vertakking u wilt maken, zoals ***/master**.
4. Configureer uw *GitHub* (waar de opslagplaats wordt gehost), zodat deze kan communiceren met Jenkins. Volg daarvoor deze stappen:
  1. Ga naar de pagina van de GitHub-opslagplaats. Ga naar **Instellingen** -> **Integraties en services**.
  2. Selecteer **Service toevoegen**, typ Jenkins en selecteer de **Jenkins-Github-invoegtoepassing**.
  3. Geef de Jenkins-webhook-URL op (standaard is dit ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klik op Service toevoegen/bijwerken.
  4. Er wordt een testgebeurtenis verzonden naar uw Jenkins-exemplaar. Er komt een groen vinkje bij de webhook te staan in GitHub en uw project wordt gemaakt!
  5. In het gedeelte **Triggers bouwen** selecteert u welke bouwoptie u wilt gebruiken. In dit geval activeert u een bouwgebeurtenis wanneer er wordt gepusht naar de opslagplaats. De bijbehorende optie is dan dus **GitHub-hookactivatie voor GITScm-polling**. (Voorheen heette dit 'Bouwen wanneer een wijziging naar GitHub wordt gepusht'.)
  6. Selecteer in het **gedeelte Bouwen** in de vervolgkeuzelijst **Stap voor bouwen toevoegen** de optie **Gradle Script aanroepen**. In de widget die wordt geopend, geeft u voor uw toepassing het pad op naar **Hoofdscript**. build.gradle wordt opgehaald via het opgegeven pad. Als u een project maakt met de naam ``MyActor`` (met behulp van de invoegtoepassing Eclipse of de Yeoman-generator), moet het hoofdscript ``${WORKSPACE}/MyActor`` bevatten. Deze sectie ziet er (als voorbeeld) als volgt uit -

    ![Service Fabric-bouwactie voor Jenkins][build-step]
  7. Selecteer in de vervolgkeuzelijst **Acties na bouwen** de optie **Service Fabric-project implementeren**. U moet hier clustergegevens opgeven, zoals waar de via Jenkins opgestelde Service Fabric-toepassing wordt geïmplementeerd en andere toepassingsgegevens die nodig zijn voor het implementeren van de toepassing. De volgende schermafbeelding kan als uitgangspunt worden gebruikt:

    ![Service Fabric-bouwactie voor Jenkins][post-build-step]

 > [!NOTE]
 > Het cluster dat u hier gebruikt, moet het cluster zijn waarin de Jenkins-containertoepassing wordt gehost als u Service Fabric gebruikt om de Jenkins-containerinstallatiekopie te implementeren.
 >

### <a name="end-to-end-scenario"></a>End-to-end-scenario
GitHub en Jenkins zijn nu geconfigureerd. U kunt een voorbeeldwijziging aanbrengen aan uw project ``MyActor`` in de opslagplaats, zoals https://github.com/sayantancs/SFJenkins. Push uw wijzigingen daarna naar de externe ``master``-vertakking (of een vertakking die u zodanig hebt geconfigureerd dat u ermee kunt werken). Op die manier wordt de Jenkins-taak ``MyJob`` die u hebt geconfigureerd, geactiveerd. Hiermee worden de wijzigingen opgehaald van GitHub. Daarna worden ze gebouwd en wordt de toepassing geïmplementeerd in het clustereindpunt dat u hebt opgegeven voor de acties ná het bouwen.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

