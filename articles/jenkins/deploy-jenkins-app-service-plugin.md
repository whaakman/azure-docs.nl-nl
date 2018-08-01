---
title: Implementeren in Azure App Service met behulp van de Jenkins-invoegtoepassing
description: Informatie over het gebruik van de Azure App Service Jenkins-invoegtoepassing een Java-web-app implementeren in Azure in Jenkins
ms.topic: article
ms.author: tarcher
author: tomarcher
manager: jpconnock
ms.service: devops
ms.custom: jenkins
ms.date: 07/31/2018
ms.openlocfilehash: 1b01eb760fa36c9f0fb6180c12dc3e5c1bf9de6f
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391483"
---
# <a name="deploy-to-azure-app-service-by-using-the-jenkins-plugin"></a>Implementeren in Azure App Service met behulp van de Jenkins-invoegtoepassing 

Als u wilt een Java-web-app implementeren in Azure, kunt u de Azure CLI in [Jenkins pijplijn](/azure/jenkins/execute-cli-jenkins-pipeline) of kunt u de [Azure App Service Jenkins-invoegtoepassing](https://plugins.jenkins.io/azure-app-service). De Jenkins-invoegtoepassing voor versie 1.0 ondersteunt doorlopende implementatie met behulp van de functie Web Apps van Azure App Service via:
* GIT- of FTP.
* Docker voor Web App on Linux.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Jenkins voor het implementeren van Web-Apps via Git of FTP configureren.
> * Jenkins voor het implementeren van Web App for Containers configureren.

## <a name="create-and-configure-a-jenkins-instance"></a>Maken en configureren van een Jenkins-exemplaar

Als u nog een Jenkins-Master hebt, begint u met de [oplossingssjabloon](install-jenkins-solution-template.md), waaronder Java Development Kit (JDK) versie 8 en de volgende vereiste Jenkins invoegtoepassingen:

* [Client-invoegtoepassing voor Jenkins Git](https://plugins.jenkins.io/git-client) versie 2.4.6 
* [Docker Commons invoegtoepassing](https://plugins.jenkins.io/docker-commons) versie 1.4.0
* [Azure-referenties](https://plugins.jenkins.io/azure-credentials) versie 1.2
* [Azure App Service](https://plugins.jenkins.io/azure-app-service) versie 0.1

De Jenkins-invoegtoepassing kunt u een WebApp implementeren in elke taal die wordt ondersteund door Web-Apps, zoals C#, PHP, Java en Node.js. In deze zelfstudie gebruiken we een [eenvoudige Java-web-app voor Azure](https://github.com/azure-devops/javawebappsample). Als u wilt splitsen in de opslagplaats naar uw eigen GitHub-account, selecteert u de **Fork** knop in de rechterbovenhoek van de GitHub-interface.  
> [!NOTE]
> De Java JDK en Maven zijn vereist om de Java-project te bouwen. Deze onderdelen op de Jenkins-Master, of op de VM-agent installeren als u de agent voor continue integratie. 

Voor het installeren van de onderdelen, meld u aan bij de Jenkins-exemplaar met SSH en voer de volgende opdrachten uit:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Als u wilt implementeren naar Web App for Containers, Docker te installeren op de Jenkins-Master of op de VM-agent die wordt gebruikt voor de build. Zie voor instructies [installeren Docker op Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntu/).

##<a name="service-principal"></a> Een Azure service-principal toevoegen aan de Jenkins-referenties

U moet een Azure service-principal in Azure implementeren. 


1. U kunt een Azure service-principal maken met de [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) of de [Azure-portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal).
2. Selecteer op het dashboard van Jenkins **referenties** > **System**. Selecteer **globale credentials(unrestricted)**.
3. Als u wilt toevoegen van een Microsoft Azure service-principal, selecteer **referenties toevoegen**. Geef waarden voor de **abonnements-ID**, **Client-ID**, **Clientgeheim**, en **OAuth 2.0 Token Endpoint** velden. Stel de **ID** veld **mySp**. We gebruiken deze ID in de volgende stappen in dit artikel.


## <a name="configure-jenkins-to-deploy-web-apps-by-uploading-files"></a>Configureren van Jenkins voor het implementeren van Web-Apps door bestanden te uploaden

U kunt uw build-artefacten (bijvoorbeeld een WAR-bestand in Java) voor het implementeren van uw project naar Web-Apps, uploaden met behulp van Git of FTP.

Voordat u de taak in Jenkins instelt, moet u een Azure App Service-plan en een web-app om uit te voeren van de Java-app.


1. Maak een Azure App Service-plan met de **gratis** prijscategorie uit te voeren met behulp van de `az appservice plan create` [Azure CLI-opdracht](/cli/azure/appservice/plan#az_appservice_plan_create). De App Service-plan definieert de fysieke resources die worden gebruikt voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een App Service-plan delen deze resources. Gedeelde resources helpen u om op te slaan op de kosten als u meerdere apps host.
2. Maak een web-app. U kunt de [Azure-portal](/azure/app-service-web/web-sites-configure) of de volgende `az` Azure CLI-opdracht:
    ```azurecli-interactive 
    az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
    ```
    
3. Instellen van de configuratie van de Java-runtime die uw app nodig heeft. De volgende Azure CLI-opdracht configureert de web-app uit te voeren op een recente JDK 8 en [Apache Tomcat](http://tomcat.apache.org/) versie 8.0:
    ```azurecli-interactive
    az webapp config set \
    --name <myAppName> \
    --resource-group <myResourceGroup> \
    --java-version 1.8 \
    --java-container Tomcat \
    --java-container-version 8.0
    ```

### <a name="set-up-the-jenkins-job"></a>Instellen van de Jenkins-taak

1. Maak een nieuwe **freestyle** project op het Dashboard van Jenkins.
2. Configureer de **Source Code Management** veld voor het gebruik van de lokale fork van de [eenvoudige Java-web-app voor Azure](https://github.com/azure-devops/javawebappsample). Geef de **opslagplaats-URL** waarde. Bijvoorbeeld: http://github.com/&lt; your_ID > / javawebappsample.
3. Toevoegen van een stap voor het bouwen van het project met behulp van Maven door toe te voegen de **shell uitvoeren** opdracht. In dit voorbeeld moet een extra opdracht naam wijzigen van de \*WAR-bestand in de doelmap naar **ROOT.war**:   
    ```bash
    mvn clean package
    mv target/*.war target/ROOT.war
    ```

4. Een actie na bouwen toevoegen door te selecteren **publiceren van een Azure-Web-App**.
5. Geef **mySp** als de Azure-service-principal. Deze principal is opgeslagen als de [Azure Credentials](#service-principal) in de vorige stap.
6. In de **Appconfiguratie** sectie, kiest u de resource-groep en web-app in uw abonnement. De Jenkins-invoegtoepassing detecteert automatisch of de web-app is gebaseerd op Windows of Linux. Voor een Windows-web-app, de **bestanden publiceren** optie wordt weergegeven.
7. Vul in de bestanden die u wilt implementeren. Geef het WAR-pakket bijvoorbeeld, als u Java. Gebruik het optionele **bronmap** en **doelmap** parameters om op te geven van de bron- en mappen die u wilt gebruiken voor het bestand uploaden. De Java-web-app in Azure wordt uitgevoerd in een Tomcat-server. Dus voor Java uploadt u het WAR-pakket naar de map Web-Apps. Voor dit voorbeeld stelt u de **bronmap** waarde die moet worden **doel** en de **doelmap** waarde die moet worden **webapps**.
8. Als u implementeren op een site dan productie wilt, kunt u ook instellen de **sleuf** naam.
9. Sla het project en bouw het. Uw web-app is geïmplementeerd in Azure als de build voltooid is.

### <a name="deploy-web-apps-by-uploading-files-using-jenkins-pipeline"></a>Web-Apps implementeren met het uploaden van bestanden met behulp van Jenkins-pijplijn

De Azure App Service Jenkins-invoegtoepassing is gereed is voor pijplijn. U kunt verwijzen naar het volgende voorbeeld in de GitHub-opslagplaats.

1. Open in de interface van GitHub, de **Jenkinsfile_ftp_plugin** bestand. Als u wilt bewerken in het bestand, selecteer het potloodpictogram. Update de **resourceGroup** en **webAppName** definities voor uw web-app op 11 en 12, respectievelijk regels:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Stel de **withCredentials** definitie op regel 14 van de referentie-ID in uw Jenkins-exemplaar:
    ```java
    withCredentials([azureServicePrincipal('<mySp>')]) {
    ```

### <a name="create-a-jenkins-pipeline"></a>Een Jenkins-pijplijn maken

1. Jenkins in een webbrowser openen. Selecteer **Nieuw Item**.
2. Geef een naam op voor de taak en selecteer **pijplijn**. Selecteer **OK**.
3. Selecteer de **pijplijn** tabblad.
4. Voor de **definitie** waarde, selecteer **Pipeline-script uit SCM**.
5. Voor de **SCM** waarde, selecteer **Git**. Voer de GitHub-URL voor uw Gevorkte opslagplaats. Bijvoorbeeld: https://&lt;your_forked_repo > .git.
6. Update de **scriptpad** waarde die moet worden **Jenkinsfile_ftp_plugin**.
7. Selecteer **opslaan** en voer de taak.

## <a name="configure-jenkins-to-deploy-web-app-for-containers"></a>Jenkins voor het implementeren van Web App for Containers configureren

Web App on Linux biedt ondersteuning voor implementatie met behulp van Docker. Voor het implementeren van uw web-app met behulp van Docker, moet u een docker-bestand dat uw web-app met een service-runtime-in een Docker-installatiekopie pakketten opgeven. De Jenkins-invoegtoepassing vervolgens de installatiekopie is gebaseerd, wordt naar een Docker-register gepusht en implementeert u de installatiekopie naar uw web-app.

Web App on Linux biedt ook ondersteuning voor traditionele implementatiemethoden, zoals Git en FTP-, maar alleen voor ingebouwde talen (.NET Core, Node.js, PHP en Ruby). Voor andere talen moet u uw code en service runtime voor de toepassing samen te verpakken in een Docker-installatiekopie en Docker gebruiken om te implementeren.

Voordat u de taak in Jenkins instelt, moet u een web-app op Linux. U moet ook een containerregister opslaan en beheren van uw persoonlijke Docker-containerinstallatiekopieën. U kunt DockerHub gebruiken om het containerregister te maken. In dit voorbeeld gebruiken we Azure Container Registry.

* [Uw web-app maken in Linux](../app-service/containers/quickstart-nodejs.md).
* Azure Container Registry is een beheerde [Docker-register](https://docs.docker.com/registry/) service die is gebaseerd op de open-source Docker Registry versie 2.0. [Maak een Azure container registry](/azure/container-registry/container-registry-get-started-azure-cli). U kunt ook DockerHub gebruiken.

### <a name="set-up-the-jenkins-job-for-docker"></a>Instellen van de Jenkins-taak voor Docker

1. Maak een nieuwe **freestyle** project op het Dashboard van Jenkins.
2. Configureer de **Source Code Management** veld voor het gebruik van de lokale fork van de [eenvoudige Java-web-app voor Azure](https://github.com/azure-devops/javawebappsample). Geef de **opslagplaats-URL** waarde. Bijvoorbeeld: http://github.com/&lt; your_ID > / javawebappsample.
3. Toevoegen van een stap voor het bouwen van het project met behulp van Maven door toe te voegen een **shell uitvoeren** opdracht. Neem de volgende regel in de opdracht:
    ```bash
    mvn clean package
    ```

4. Een actie na bouwen toevoegen door te selecteren **publiceren van een Azure-Web-App**.
5. Geef **mySp** als de Azure-service-principal. Deze principal is opgeslagen als de [Azure Credentials](#service-principal) in de vorige stap.
6. In de **Appconfiguratie** sectie, kiest u de resourcegroep en een Linux-web-app in uw abonnement.
7. Kies **publiceren via Docker**.
8. Vul de **Dockerfile** padwaarde. U kunt de standaard waarde /Dockerfile houden.
Voor de **Docker-register URL** waarde, de URL opgeven met behulp van de indeling https://&lt;yourRegistry >. azurecr.io als u Azure Container Registry gebruikt. Als u DockerHub gebruikt, laat u de waarde leeg.
9. Voor de **registerreferenties** waarde, het toevoegen van de referentie voor het containerregister. U krijgt de gebruikers-id en het wachtwoord door het uitvoeren van de volgende opdrachten in de Azure CLI. De eerste opdracht wordt het administrator-account.
    ```azurecli-interactive
    az acr update -n <yourRegistry> --admin-enabled true
    az acr credential show -n <yourRegistry>
    ```

10. De Docker-installatiekopie naam en het label waarde in de **Geavanceerd** tabblad zijn optioneel. Standaard de waarde voor de naam van de installatiekopie wordt opgehaald uit de naam van de installatiekopie die u hebt geconfigureerd in de Azure portal in de **Docker-Container** instelling. De code is gegenereerd op basis van $BUILD_NUMBER.
    > [!NOTE]
    > Zorg ervoor dat u de naam van de installatiekopie opgeven in de Azure-portal of geef een **Docker-installatiekopie** waarde in de **Geavanceerd** tabblad. Voor dit voorbeeld stelt u de **Docker-installatiekopie** waarde die moet worden &lt;your_Registry >.azurecr.io/calculator en laat de **Docker afbeeldingscode** waarde leeg.

11. De implementatie mislukt als u een ingebouwde Docker-installatiekopie-instelling gebruiken. Wijzigen van de configuratie van de Docker voor het gebruik van een aangepaste installatiekopie in de **Docker-Container** instellen in Azure portal. Gebruik de aanpak voor het uploaden van bestand om te implementeren voor een ingebouwde installatiekopie.
12. Net als bij de aanpak voor het uploaden van bestand, kunt u een andere **sleuf** naam dan **productie**.
13. Opslaan en bouw het project. Om de containerinstallatiekopie naar het register is gepusht en de web-app wordt geïmplementeerd.

### <a name="deploy-web-app-for-containers-by-using-jenkins-pipeline"></a>Web App for Containers implementeren met behulp van Jenkins-pijplijn

1. Open in de interface van GitHub, de **Jenkinsfile_container_plugin** bestand. Als u wilt bewerken in het bestand, selecteer het potloodpictogram. Update de **resourceGroup** en **webAppName** definities voor uw web-app op 11 en 12, respectievelijk regels:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Wijzig regel 13 in uw container registry-server:
    ```java
    def registryServer = '<registryURL>'
    ```

3. Wijzig regel 16 het gebruik van de referentie-ID in uw Jenkins-exemplaar:
    ```java
    azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
    ```

### <a name="create-a-jenkins-pipeline"></a>Een Jenkins-pijplijn maken    

1. Jenkins in een webbrowser openen. Selecteer **Nieuw Item**.
2. Geef een naam op voor de taak en selecteer **pijplijn**. Selecteer **OK**.
3. Selecteer de **pijplijn** tabblad.
4. Voor de **definitie** waarde, selecteer **Pipeline-script uit SCM**.
5. Voor de **SCM** waarde, selecteer **Git**. Voer de GitHub-URL voor uw Gevorkte opslagplaats. Bijvoorbeeld: https://&lt;your_forked_repo > .git.
7. Update de **scriptpad** waarde die moet worden **Jenkinsfile_container_plugin**.
8. Selecteer **opslaan** en voer de taak.

## <a name="verify-your-web-app"></a>Controleer of uw web-app

1. Om te controleren dat het WAR-bestand wordt geïmplementeerd op uw web-app, open een webbrowser.
2. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Vervang &lt;your_app_name > met de naam van uw web-app. U ziet het bericht:
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jun 17 16:39:10 UTC 2017
    ```

3. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Vervang &lt;x > en &lt;y > met willekeurige getallen om op te halen van de som van de x + y. Geeft de som van de calculator: ![Calculator: toevoegen](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-azure-app-service-on-linux"></a>Voor Azure App Service onder Linux

1. Als u wilt controleren of uw web-app, voer de volgende opdracht in de Azure CLI:
    ```CLI
    az acr repository list -n <myRegistry> -o json
    ```
    Het volgende bericht wordt weergegeven:
    ```CLI
    ["calculator"]
    ```
    
2. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Vervang &lt;your_app_name > met de naam van uw web-app. U ziet het bericht: 
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jul 09 16:39:10 UTC 2017
    ```

3. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Vervang &lt;x > en &lt;y > met willekeurige getallen om op te halen van de som van de x + y.
    
## <a name="troubleshooting-the-jenkins-plugin"></a>De Jenkins-invoegtoepassing oplossen

Als u er fouten met de Jenkins-invoegtoepassingen optreden, dient u een probleem in de [Jenkins JIRA](https://issues.jenkins-ci.org/) voor het specifieke onderdeel.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u de invoegtoepassing voor Azure App Service-Jenkins implementeren in Azure.

U hebt geleerd hoe u:

> [!div class="checklist"]
> * Jenkins voor het implementeren van Azure App Service via FTP configureren 
> * Jenkins implementeren naar Web App for Containers configureren 
