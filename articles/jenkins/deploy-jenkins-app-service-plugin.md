---
title: Implementeren in Azure App Service met behulp van de invoegtoepassing Jenkins | Microsoft Docs
description: Informatie over het gebruik van de Azure App Service Jenkins-invoegtoepassing voor het implementeren van een Java-web-app in Azure in Jenkins
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 9b79e3b498e51e626e7e9a87d2bb1a66366acff5
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="deploy-to-azure-app-service-by-using-the-jenkins-plugin"></a>Implementeren in Azure App Service met behulp van de invoegtoepassing Jenkins 

Als u wilt een Java-web-app implementeren in Azure, kunt u de Azure CLI in [Jenkins pijplijn](/azure/jenkins/execute-cli-jenkins-pipeline) of kunt u de [invoegtoepassing van Azure App Service Jenkins](https://plugins.jenkins.io/azure-app-service). De Jenkins versie 1.0 van de invoegtoepassing biedt ondersteuning voor continue implementatie met behulp van de functie Web Apps van Azure App Service via:
* GIT of FTP.
* Docker voor Web-Apps op Linux.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Configureer Jenkins voor het implementeren van Web-Apps via Git- of FTP.
> * Configureer Jenkins Web-App voor Containers implementeren.

## <a name="create-and-configure-a-jenkins-instance"></a>Maken en configureren van een exemplaar Jenkins

Als u een model Jenkins nog geen hebt, begint u met de [oplossingssjabloon](install-jenkins-solution-template.md), waaronder Java Development Kit (JDK) versie 8 en de volgende vereiste Jenkins invoegtoepassingen:

* [De invoegtoepassing client Jenkins Git](https://plugins.jenkins.io/git-client) versie 2.4.6 
* [Docker Commons invoegtoepassing](https://plugins.jenkins.io/docker-commons) versie 1.4.0
* [Azure-referenties](https://plugins.jenkins.io/azure-credentials) versie 1.2
* [Azure App Service](https://plugins.jenkins.io/azure-app-server) versie 0.1

U kunt de invoegtoepassing Jenkins gebruiken voor het implementeren van een web-app in elke taal die wordt ondersteund door Web-Apps, zoals C#, PHP, Java en Node.js. In deze zelfstudie gebruiken we een [eenvoudige Java-web-app voor Azure](https://github.com/azure-devops/javawebappsample). Om te vertakken de opslagplaats op uw eigen GitHub-account, selecteert u de **Fork** knop in de rechterbovenhoek van de GitHub-interface.  
> [!NOTE]
> De Java-JDK en Maven zijn vereist om de Java-project te bouwen. Deze onderdelen op de Jenkins Master of op de VM-agent installeren als u de agent voor continue integratie. 

De onderdelen installeren, meld u aan het exemplaar Jenkins met SSH bij en voer de volgende opdrachten:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Als u wilt implementeren op Web-App voor Containers, installeert u Docker op Jenkins Master of de VM-agent die wordt gebruikt voor de build. Zie voor instructies [Docker installeren op Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntu/).

##<a name="service-principal"></a>Een Azure-service-principal toevoegen aan de Jenkins-referenties

U moet een Azure-service-principal te implementeren in Azure. 


1. Gebruik voor het maken van een Azure service-principal de [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) of de [Azure-portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal).
2. Selecteer op het dashboard Jenkins **referenties** > **System**. Selecteer **globale credentials(unrestricted)**.
3. Selecteer om een Microsoft Azure service-principal toe **referenties toevoegen**. Geef waarden voor de **abonnements-ID**, **Client-ID**, **Clientgeheim**, en **OAuth 2.0-Tokeneindpunt** velden. Stel de **ID** veld **mySp**. We gebruiken deze ID in de volgende stappen in dit artikel.


## <a name="configure-jenkins-to-deploy-web-apps-by-uploading-files"></a>Jenkins voor het implementeren van Web-Apps door het uploaden van bestanden configureren

Als u wilt uw project op Web-Apps implementeert, kunt u uw build-artefacten (bijvoorbeeld een WAR-bestand in Java) met behulp van Git- of FTP-uploaden.

Voordat u de taak in Jenkins hebt ingesteld, moet u een Azure App Service-abonnement en een web-app voor het uitvoeren van de Java-app.


1. Maken van een Azure App Service-abonnement met de **vrije** prijscategorie met behulp van de `az appservice plan create` [Azure CLI-opdracht](/cli/azure/appservice/plan#create). De App Service-abonnement definieert de fysieke resources die worden gebruikt voor het hosten van uw apps. Alle toepassingen die zijn toegewezen aan een App Service-abonnement kunt u deze resources delen. Gedeelde resources helpen u op te slaan op kosten bij het hosten van meerdere apps.
2. Maak een web-app. U kunt de [Azure-portal](/azure/app-service-web/web-sites-configure) of de volgende `az` Azure CLI-opdracht:
    ```azurecli-interactive 
    az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
    ```
    
3. De Java runtime-configuratie die uw app moet instellen. De volgende opdracht in de Azure CLI configureert u de web-app uit te voeren op een recente JDK 8 en [Apache Tomcat](http://tomcat.apache.org/) versie 8.0:
    ```azurecli-interactive
    az webapp config set \
    --name <myAppName> \
    --resource-group <myResourceGroup> \
    --java-version 1.8 \
    --java-container Tomcat \
    --java-container-version 8.0
    ```

### <a name="set-up-the-jenkins-job"></a>De taak Jenkins instellen

1. Maak een nieuwe **freestyle** project op het Jenkins Dashboard.
2. Configureer de **Source Code Management** veld dat wordt gebruikt van uw lokale fork van de [eenvoudige Java-web-app voor Azure](https://github.com/azure-devops/javawebappsample). Geef de **URL opslagplaats** waarde. Bijvoorbeeld: http://github.com/&lt;your_ID > / javawebappsample.
3. Toevoegen van een stap om het project bouwen met behulp van Maven door toe te voegen de **shell uitvoeren** opdracht. Bijvoorbeeld, moeten we een extra opdracht naam wijzigen van de \*.war-bestand in de doelmap naar **ROOT.war**:   
    ```bash
    mvn clean package
    mv target/*.war target/ROOT.war
    ```

4. Een actie na build toevoegen door te selecteren **een Azure-Web-App publiceren**.
5. Geef **mySp** als de Azure-service-principal. Deze principal is opgeslagen als de [Azure-referenties](#service-principal) in de vorige stap.
6. In de **App-configuratie** sectie, kiest u de resource-groep en web-app in uw abonnement. De invoegtoepassing Jenkins detecteert automatisch of de web-app is gebaseerd op Windows of Linux. Voor een web-app van Windows de **bestanden publiceren** optie wordt weergegeven.
7. Vul in de bestanden die u wilt implementeren. Geef het WAR-pakket bijvoorbeeld, als u Java. Gebruik het optionele **bronmap** en **doelmap** parameters om op te geven van de bron en doel-mappen moet worden gebruikt voor het uploaden van het bestand. De Java-web-app in Azure wordt uitgevoerd in een Tomcat-server. Dus voor Java uploadt u het WAR-pakket naar de map webapps. Bijvoorbeeld, stel de **bronmap** van waarde naar **doel** en de **doelmap** van waarde naar **webapps**.
8. Als u implementeren in een sleuf dan productie wilt, kunt u ook instellen de **sleuf** naam.
9. Sla het project en bouw het. Uw web-app wordt geïmplementeerd naar Azure wanneer de build voltooid is.

### <a name="deploy-web-apps-by-uploading-files-using-jenkins-pipeline"></a>Web-Apps implementeren met het uploaden van bestanden met behulp van Jenkins pijplijn

De invoegtoepassing van Azure App Service Jenkins is pijplijn gereed. U kunt verwijzen naar het volgende voorbeeld in de GitHub-opslagplaats.

1. Open in de GitHub-interface, de **Jenkinsfile_ftp_plugin** bestand. Selecteer het bestand bewerken, het potloodpictogram. Update de **resourceGroup** en **webAppName** 11 tot en met 12 respectievelijk definities voor uw web-app op regels:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Stel de **withCredentials** definitie op regel 14 op de referentie-ID in uw exemplaar Jenkins:
    ```java
    withCredentials([azureServicePrincipal('<mySp>')]) {
    ```

### <a name="create-a-jenkins-pipeline"></a>Een pijplijn Jenkins maken

1. Open Jenkins in een webbrowser. Selecteer **Nieuw Item**.
2. Geef een naam op voor de taak en selecteer **pijplijn**. Selecteer **OK**.
3. Selecteer de **pijplijn** tabblad.
4. Voor de **definitie** waarde, selecteer **Pipeline-script uit SCM**.
5. Voor de **SCM** waarde, selecteer **Git**. Geef de GitHub-URL voor uw Gevorkte opslagplaats. Bijvoorbeeld: https://&lt;your_forked_repo > .git.
6. Update de **scriptpad** van waarde naar **Jenkinsfile_ftp_plugin**.
7. Selecteer **opslaan** en voer de taak.

## <a name="configure-jenkins-to-deploy-web-app-for-containers"></a>Configureer Jenkins Web-App voor Containers implementeren

Web-Apps op Linux ondersteunt de implementatie met behulp van Docker. Voor het implementeren van uw web-app met behulp van Docker, moet u een Dockerfile die uw web-app met een service-runtime worden verpakt in een Docker-installatiekopie opgeven. De invoegtoepassing Jenkins vervolgens de installatiekopie is gebaseerd, duwt deze met een Docker-register en de installatiekopie implementeert op uw web-app.

Traditionele implementatiemethoden, zoals Git en FTP-, maar alleen voor ingebouwde talen (.NET Core, Node.js, PHP en Ruby) biedt ook ondersteuning voor Web-Apps op Linux. Voor andere talen moet u uw toepassing en de service een runtime-pakket samen in een Docker-installatiekopie en Docker gebruiken om te implementeren.

Voordat u de taak in Jenkins instelt, moet u een web-app op Linux. U moet ook een container register opslaan en beheren van uw persoonlijke installatiekopieën van de Docker-container. U kunt DockerHub gebruiken voor het maken van het register van de container. In dit voorbeeld gebruiken we Azure Container register.

* [Uw web-app maken in Linux](../app-service/containers/quickstart-nodejs.md).
* Azure Container register is een beheerde [Docker-register](https://docs.docker.com/registry/) service die is gebaseerd op de open source Docker register versie 2.0. [Maken van een Azure container registry](/azure/container-registry/container-registry-get-started-azure-cli). U kunt ook DockerHub gebruiken.

### <a name="set-up-the-jenkins-job-for-docker"></a>De taak Jenkins voor Docker instellen

1. Maak een nieuwe **freestyle** project op het Jenkins Dashboard.
2. Configureer de **Source Code Management** veld dat wordt gebruikt van uw lokale fork van de [eenvoudige Java-web-app voor Azure](https://github.com/azure-devops/javawebappsample). Geef de **URL opslagplaats** waarde. Bijvoorbeeld: http://github.com/&lt;your_ID > / javawebappsample.
3. Toevoegen van een stap om het project bouwen met behulp van Maven door toe te voegen een **shell uitvoeren** opdracht. Neem de volgende regel in de opdracht:
    ```bash
    mvn clean package
    ```

4. Een actie na build toevoegen door te selecteren **een Azure-Web-App publiceren**.
5. Geef **mySp** als de Azure-service-principal. Deze principal is opgeslagen als de [Azure-referenties](#service-principal) in de vorige stap.
6. In de **App-configuratie** sectie, kiest u de resourcegroep en een Linux-web-app in uw abonnement.
7. Kies **publiceren via de Docker**.
8. Vul de **Dockerfile** padwaarde. U kunt de standaard waarde /Dockerfile houden.
Voor de **Docker register URL** waarde: de URL opgeven met behulp van de indeling https://&lt;yourRegistry >. azurecr.io als u gebruikmaakt van Azure Container register. Als u DockerHub gebruikt, laat u de waarde leeg.
9. Voor de **register referenties** waarde: de referentie voor het register van de container toevoegen. U kunt de gebruikersnaam en wachtwoord ophalen met de volgende opdrachten in de Azure CLI. De eerste opdracht wordt het administrator-account.
    ```azurecli-interactive
    az acr update -n <yourRegistry> --admin-enabled true
    az acr credential show -n <yourRegistry>
    ```

10. Docker naam en de tag waarde voor de installatiekopie in de **Geavanceerd** tabblad zijn optioneel. De waarde voor de naam van de installatiekopie wordt standaard opgehaald uit de naam van de installatiekopie die u hebt geconfigureerd in de Azure Portal de **Docker-Container** instelling. De code is gegenereerd op basis van $BUILD_NUMBER.
    > [!NOTE]
    > Zorg ervoor dat de naam van de installatiekopie opgeven in de Azure portal of geef een **Docker-afbeelding** waarde in de **Geavanceerd** tabblad. Instellen voor dit voorbeeld de **Docker-afbeelding** van waarde naar &lt;your_Registry >.azurecr.io/calculator en laat de **Docker afbeeldingscode** waarde is leeg.

11. De implementeren mislukt als u een ingebouwde Docker installatiekopie-instelling gebruiken. Wijzig de Docker-configuratie voor het gebruik van een aangepaste installatiekopie in de **Docker-Container** instellen in de Azure-portal. Gebruik de benadering voor het uploaden van bestand om te implementeren voor een ingebouwde installatiekopie.
12. Net als bij de aanpak van bestand uploaden, kunt u een andere **sleuf** naam dan **productie**.
13. Sla en bouw het project. Uw installatiekopie container wordt doorgeschoven, toe aan het register en de web-app wordt geïmplementeerd.

### <a name="deploy-web-app-for-containers-by-using-jenkins-pipeline"></a>Web-App voor Containers implementeren met behulp van Jenkins-Pipeline

1. Open in de GitHub-interface, de **Jenkinsfile_container_plugin** bestand. Selecteer het bestand bewerken, het potloodpictogram. Update de **resourceGroup** en **webAppName** 11 tot en met 12 respectievelijk definities voor uw web-app op regels:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Regel 13 wijzigen in de container register-server:
    ```java
    def registryServer = '<registryURL>'
    ```

3. Regel 16 bij gebruik van de referentie-ID in uw exemplaar Jenkins wijzigen:
    ```java
    azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
    ```

### <a name="create-a-jenkins-pipeline"></a>Een pijplijn Jenkins maken    

1. Open Jenkins in een webbrowser. Selecteer **Nieuw Item**.
2. Geef een naam op voor de taak en selecteer **pijplijn**. Selecteer **OK**.
3. Selecteer de **pijplijn** tabblad.
4. Voor de **definitie** waarde, selecteer **Pipeline-script uit SCM**.
5. Voor de **SCM** waarde, selecteer **Git**. Geef de GitHub-URL voor uw Gevorkte opslagplaats. Bijvoorbeeld: https://&lt;your_forked_repo > .git.
7. Update de **scriptpad** van waarde naar **Jenkinsfile_container_plugin**.
8. Selecteer **opslaan** en voer de taak.

## <a name="verify-your-web-app"></a>Controleer of uw web-app

1. Om te verifiëren dat het WAR-bestand wordt geïmplementeerd op uw web-app, open een webbrowser.
2. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Vervang &lt;your_app_name > met de naam van uw web-app. U ziet het bericht:
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jun 17 16:39:10 UTC 2017
    ```

3. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Vervang &lt;x > en &lt;y > met willekeurige getallen om op te halen van de som van x + y. Bevat de som van de Rekenmachine: ![Rekenmachine: toevoegen](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-azure-app-service-on-linux"></a>Voor op Linux-Azure App Service

1. Om te controleren of uw web-app, voer de volgende opdracht in de Azure CLI:
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

3. Ga naar http://&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Vervang &lt;x > en &lt;y > met willekeurige getallen om op te halen van de som van x + y.
    
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u de invoegtoepassing van Azure App Service Jenkins om te implementeren in Azure.

U hebt geleerd hoe u:

> [!div class="checklist"]
> * Jenkins voor het implementeren van Azure App Service via FTP configureren 
> * Jenkins om te implementeren voor Web-App voor Containers configureren 
