---
title: Continue build en de integratie voor uw Azure Service Fabric Linux-toepassingen met behulp van Jenkins | Microsoft Docs
description: Continue build en integratie voor uw Service Fabric Linux-toepassing met behulp van Jenkins
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: saysa
ms.openlocfilehash: 80c52cfeab007030203b6af4bb220f1a847e9426
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Jenkins gebruiken om te bouwen en implementeren van uw Linux-toepassingen
Jenkins is een populair hulpprogramma voor doorlopende integratie en implementatie van uw apps. Hier leest u hoe u een Azure Service Fabric-toepassing maakt en implementeert met behulp van Jenkins.

## <a name="general-prerequisites"></a>Algemene vereisten
- Zorg ervoor dat Git lokaal is geïnstalleerd. U kunt de juiste Git-versie (deze is afhankelijk van uw besturingssysteem) installeren via [de Git-downloadpagina](https://git-scm.com/downloads). Als u niet bekend bent met Git, raadpleegt u de [Git-documentatie](https://git-scm.com/docs) voor meer informatie.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins instellen in een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. De volgende secties ziet het instellen van binnen een cluster tijdens het gebruik van een Azure storage-account voor het opslaan van de status van het exemplaar van de container.

### <a name="prerequisites"></a>Vereisten
1. Zorg ervoor dat u beschikt over een Service Fabric-cluster voor Linux. Voor een Service Fabric-cluster dat is gemaakt via Azure Portal, is Docker al geïnstalleerd. Als u het cluster lokaal uitvoert, controleert u met behulp van de opdracht ``docker info`` of Docker is geïnstalleerd. Als het niet is geïnstalleerd, installeert u het dienovereenkomstig met de volgende opdrachten:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ``` 

   > [!NOTE]
   > Zorg ervoor dat de poort 8081 is opgegeven als een aangepaste eindpunt op het cluster.
   >

2. Klonen van de toepassing met behulp van de volgende stappen uit:
  ```sh
  git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
  cd service-fabric-java-getting-started/Services/JenkinsDocker/
  ```

3. De status van de container Jenkins in een bestandsshare behouden:
  * Maken van een Azure storage-account in de **dezelfde regio** als uw cluster met een naam zoals ``sfjenkinsstorage1``.
  * Maak een **bestandsshare** onder de storage-Account met een naam, zoals ``sfjenkins``.
  * Klik op **Connect** voor de bestandsshare en noteer de waarden wordt weergegeven onder **verbinding te maken van Linux**, de waarde moet er ongeveer als hieronder:
  ```sh
  sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
  ```

  > [!NOTE]
  > Koppelpunt cifs shares moet u beschikken over de cifs-utils-pakket geïnstalleerd in de clusterknooppunten.       
  >

4. Werk de tijdelijke aanduiding voor waarden in de ```setupentrypoint.sh``` script met de azure-opslag-details van stap 3.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
  * Vervang ``[REMOTE_FILE_SHARE_LOCATION]`` met de waarde ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` uit de uitvoer van de verbinding te maken in stap 3 hierboven.
  * Vervang ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` met de waarde ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` uit stap 3 hierboven.

5. **Alleen beveiligde Cluster:** voordat de implementatie van toepassingen op een beveiligde cluster met Jenkins configureren, moet het certificaat in de container Jenkins toegankelijk zijn. Op Linux-clusters, de certificates(PEM) gewoon gekopieerd uit de store naar de container door X509StoreName is opgegeven. Dit certificaatverwijzing toevoegen in de ApplicationManifest onder ContainerHostPolicies en werk de vingerafdrukwaarde. De vingerafdrukwaarde moet zijn dat van een certificaat dat zich op het knooppunt bevindt.
  ```xml
  <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
  ```
  > [!NOTE]
  > De vingerafdrukwaarde moet hetzelfde zijn als het certificaat dat wordt gebruikt voor verbinding met het beveiligde cluster. 
  >

6. Verbinding maken met het cluster en installeer de containertoepassing.

  **Beveiligde Cluster**
  ```sh
  sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
  bash Scripts/install.sh
  ```

  **Niet-beveiligde Cluster**
  ```sh
  sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
  bash Scripts/install.sh
  ```

  Hiermee wordt een Jenkins-container in het cluster geïnstalleerd. Dit kan worden bewaakt met de Service Fabric Explorer.

    > [!NOTE]
    > Het duurt een paar minuten voor de installatiekopie Jenkins worden gedownload op het cluster.
    >

### <a name="steps"></a>Stappen
1. Ga in uw browser naar ``http://PublicIPorFQDN:8081``. Hier vindt u het pad naar het eerste beheerderswachtwoord dat nodig is om u aan te melden. 
2. Bekijk de Service Fabric Explorer om te bepalen welk knooppunt de container Jenkins wordt uitgevoerd. Secure Shell (SSH) aanmelden bij dit knooppunt.
```sh
ssh user@PublicIPorFQDN -p [port]
``` 
3. Haal de exemplaar-id van de container op met ``docker ps -a``.
4. Meld u met Secure Shell (SSH) aan bij de container en plak het pad dat in de Jenkins-portal wordt weergegeven. Als in de portal bijvoorbeeld het pad `PATH_TO_INITIAL_ADMIN_PASSWORD` wordt getoond, voert u de volgende opdracht uit:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  ```
  ```sh
  cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the pasword value
  ```
5. Kies op de pagina Jenkins Gettting gestart de Selecteer invoegtoepassingen voor de installatie-optie, selecteert u de **geen** selectievakje in en klik op installeren.
6. Een gebruiker maken of selecteren om door te gaan als een beheerder.

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
  1. Ophalen van de installatiekopie van het Service Fabric Jenkins container: ``docker pull rapatchi/jenkins:v10``. Deze installatiekopie wordt geleverd met Service Fabric Jenkins-invoegtoepassing die vooraf zijn geïnstalleerd.
  2. Voer de installatiekopie van de container uit: ``docker run -itd -p 8080:8080 rapatchi/jenkins:v10``
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


## <a name="create-and-configure-a-jenkins-job"></a>Een Jenkins-taak maken en configureren

1. Maak een **nieuw item** vanaf het dashboard.
2. Voer een itemnaam in (bijvoorbeeld **MyJob**). Selecteer **free-style project** en klik op **OK**.
3. Ga naar de pagina van de taak en klik op **Configure**.

   a. Selecteer het selectievakje voor in het gedeelte Algemeen **GitHub project**, en geef de URL van de GitHub-project. Dit is de URL waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins (Continue integratie, Continue implementatie), bijvoorbeeld ``https://github.com/sayantancs/SFJenkins``.

   b. Selecteer onder de sectie **Broncodebeheer** de optie **Git**. Geef de opslagplaats-URL op waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins, bijvoorbeeld ``https://github.com/sayantancs/SFJenkins.git``. U kunt hier ook opgeven welke vertakking u wilt maken, bijvoorbeeld **/master**.
4. Configureer uw *GitHub* (waar de opslagplaats wordt gehost), zodat deze kan communiceren met Jenkins. Voer de volgende stappen uit:

   a. Ga naar de pagina van de GitHub-opslagplaats. Ga naar **Instellingen** > **Integraties en services**.

   b. Selecteer **Add Service**, typ **Jenkins** en selecteer de **Jenkins-GitHub-invoegtoepassing**.

   c. Geef de Jenkins-webhook-URL op (standaard is dit ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klik op **add/update service**.

   d. Er wordt een testgebeurtenis verzonden naar uw Jenkins-exemplaar. Er komt een groen vinkje bij de webhook te staan in GitHub en uw project wordt gemaakt.

   e. Selecteer in de sectie **Build Triggers** de gewenste buildoptie. Voor dit voorbeeld wilt u een build activeren telkens wanneer er een push naar de opslagplaats plaatsvindt. Selecteer daarom de optie **GitHub hook trigger for GITScm polling**. (Voorheen heette deze optie **Build when a change is pushed to GitHub**.)

   f. **Sectie voor Java-toepassingen te bouwen:** onder de **bouwen sectie**, in de vervolgkeuzelijst **toevoegen build stap**, selecteert u de optie **aanroepen Gradle Script**. Geef in het object dat wordt geleverd opent u het menu Geavanceerd, het pad naar **hoofdmap build script** voor uw toepassing. Het build.gradle van het opgegeven pad opneemt en dienovereenkomstig werkt. Als u een project maakt met de naam ``MyActor`` (met behulp van de Eclipse-invoegtoepassing of Yeoman-generator), moet het hoofdscript ``${WORKSPACE}/MyActor`` bevatten. Bekijk de volgende schermafbeelding voor een voorbeeld van hoe dit er uitziet:

    ![Service Fabric-bouwactie voor Jenkins][build-step]

   g. **Sectie voor .net Core-toepassingen te bouwen:** onder de **bouwen sectie**, in de vervolgkeuzelijst **toevoegen build stap**, selecteert u de optie **Shell uitvoeren**. In het vak opdracht moet de map eerst moet worden gewijzigd in het pad naar het bestand build.sh. Zodra de map is gewijzigd wordt het script build.sh kan worden uitgevoerd en wordt de toepassing wordt opgebouwd.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

    Hieronder volgt een voorbeeld van de opdrachten die worden gebruikt voor het bouwen van de [teller Service](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) voorbeeld met een naam van de taak Jenkins van CounterServiceApplication.

    ![Service Fabric-bouwactie voor Jenkins][build-step-dotnet]
  
   h. Selecteer in de vervolgkeuzelijst **Post-Build Actions** de optie **Post-Build Actions**. Hier moet u clustergegevens opgeven, zoals waar de via Jenkins gecompileerde Service Fabric-toepassing wordt geïmplementeerd. Het pad naar het certificaat kan worden gevonden door de waarde van de echo Certificates_JenkinsOnSF_Code_MyCert_PEM omgevingsvariabele uit in de container echo. Dit pad kan worden gebruikt voor de sleutel van de Client en de Client Cert velden.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
    U kunt ook aanvullende toepassingsgegevens opgeven. Deze worden gebruikt om de toepassing te implementeren. Bekijk de volgende schermafbeelding voor een voorbeeld van hoe dit er uitziet:

    ![Service Fabric-bouwactie voor Jenkins][post-build-step]

      > [!NOTE]
      > Het cluster dat u hier gebruikt, moet het cluster zijn waarin de Jenkins-containertoepassing wordt gehost als u Service Fabric gebruikt om de installatiekopie van de Jenkins-containerinstallatiekopie te implementeren.
      >

## <a name="next-steps"></a>Volgende stappen
GitHub en Jenkins zijn nu geconfigureerd. U zou een aantal voorbeeldwijzigingen in uw ``MyActor``-project in de voorbeeldopslagplaats https://github.com/sayantancs/SFJenkins kunnen aanbrengen. Push de wijzigingen naar een externe ``master``-vertakking (of een andere vertakking die u hebt geconfigureerd om mee te werken). Op die manier wordt de geconfigureerde Jenkins-taak ``MyJob`` geactiveerd. Hiermee worden de wijzigingen opgehaald van GitHub. Daarna worden ze gebouwd en wordt de toepassing geïmplementeerd in het clustereindpunt dat u hebt opgegeven voor de acties ná het bouwen.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-step.png

