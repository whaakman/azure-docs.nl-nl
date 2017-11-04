---
title: Jenkins gebruiken voor het implementeren van uw web-apps in Azure | Microsoft Docs
description: Stel doorlopende integratie van GitHub in Azure App Service voor uw Java-web-apps met behulp van Jenkins en Docker.
author: rloutlaw
manager: douge
ms.service: jenkins
ms.search.scope: 
ms.devlang: java
ms.topic: article
ms.workload: web
ms.date: 08/02/2017
ms.author: routlaw
ms.custom: Jenkins, devcenter
ms.openlocfilehash: 40d7e822b586e6f6b4addcd7d4e107eda9f4ab11
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="set-up-continuous-integration-and-deployment-to-azure-app-service-with-jenkins"></a>Continue integratie en implementatie in Azure App Service met Jenkins instellen

Deze zelfstudie stelt u continue integratie en implementatie (CI/CD) van een voorbeeld-Java-web-app die is ontwikkeld met de [Spring Boot](http://projects.spring.io/spring-boot/) framework [Azure App Service Web-App op Linux](/azure/app-service/containers/app-service-linux-intro) Jenkins gebruiken.

U voert de volgende taken in deze zelfstudie:

> [!div class="checklist"]
> * Installeer de Jenkins-invoegtoepassingen die nodig zijn om te implementeren in Azure App Service.
> * Definieer een taak Jenkins Docker-installatiekopieën van een GitHub-repo-bouwen wanneer een nieuwe doorvoer wordt gepusht.
> * Definieer een nieuwe Azure-Web-App voor Linux en configureer deze als u wilt implementeren naar Azure-Container register gepusht Docker-installatiekopieën.
> * De invoegtoepassing van Azure App Service Jenkins configureren.
> * De voorbeeld-app implementeren in Azure App Service met een handmatige build.
> * Een build Jenkins activeren en de web-app bijwerken door wijzigingen pushen met GitHub.

## <a name="before-you-begin"></a>Voordat u begint

Voor deze zelfstudie hebt u het volgende nodig:

* [Jenkins](https://jenkins.io/) JDK Maven hulpprogramma's voor en geconfigureerd. Als u een systeem Jenkins geen hebt, maakt u een nu in Azure uit de [Jenkins oplossingssjabloon](/azure/jenkins/install-jenkins-solution-template).
* Een [GitHub](https://github.com) account.
* [Azure CLI 2.0](/cli/azure/overview), vanaf uw lokale opdrachtregel of in de [Azure Cloud Shell](/azure/cloud-shell/overview)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-jenkins-plug-ins"></a>Jenkins-invoegtoepassingen installeren

1. Open een webbrowser met de webconsole Jenkins en selecteer **beheren Jenkins** in het menu links, selecteer **invoegtoepassingen beheren**.
2. Selecteer de **beschikbaar** tabblad.
3. Zoek en selecteer het selectievakje naast de volgende invoegtoepassingen:   

    - [Azure App Service-invoegtoepassing](https://plugins.jenkins.io/azure-app-service)
    - [Invoegtoepassing vertakking van de GitHub-bron](https://plugins.jenkins.io/github-branch-source)

    Als de invoegtoepassingen niet weergegeven worden, controleert u of ze niet al zijn geïnstalleerd door het controleren van de **geïnstalleerde** tabblad.

1. Selecteer **nu downloaden en installeren na opnieuw opstarten** de invoegtoepassingen in uw configuratie Jenkins inschakelen.

## <a name="configure-github-and-jenkins"></a>GitHub en Jenkins configureren

Jenkins instellen voor het ontvangen van [GitHub webhooks](https://developer.github.com/webhooks/) wanneer nieuwe doorvoeracties naar een opslagplaats in uw account worden gepusht.

1. Selecteer **beheren Jenkins**, klikt u vervolgens **systeem configureren**. In de **GitHub** sectie, controleert u of **beheren hooks** is geselecteerd en selecteer vervolgens **beheren aanvullende acties voor GitHub** en kies **converteren aanmelding en wachtwoord voor het token**.
2. Selecteer de **van aanmeldingsnaam en het wachtwoord** keuzerondje en uw GitHub-gebruikersnaam en wachtwoord invoeren. Selecteer **token referenties maken** Maak een nieuwe [GitHub Personal Access Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).   
   ![GitHub PAT van aanmeldingsnaam en het wachtwoord te maken](media/jenkins-java-quickstart/create_github_credentials.png)
3.  Selecteer de zojuist gemaakte token van de **referenties** vervolgkeuzelijst in de configuratie van de GitHub-Servers. Selecteer **verbinding testen** om te controleren of de verificatie werkt.   
   ![Controleer of de verbinding met GitHub zodra PAT is geconfigureerd](media/jenkins-java-quickstart/verify_github_connection.png)

> [!NOTE]
> Als uw GitHub-account tweeledige verificatie is ingeschakeld heeft, maken van het token op GitHub en configureert Jenkins om dit te gebruiken. Controleer de [Jenkins GitHub invoegtoepassing](https://wiki.jenkins.io/display/JENKINS/Github+Plugin) documentatie voor volledige informatie.

## <a name="fork-the-sample-repo-and-create-a-jenkins-job"></a>De voorbeeld-opslagplaats vertakken en maken van een taak Jenkins 

1. Open de [Spring opstarten voorbeeld toepassing opslagplaats](https://github.com/spring-guides/gs-spring-boot-docker) en deze op uw eigen GitHub-account te vertakken door te selecteren **Fork** in de rechterbovenhoek.   
    ![Fork vanuit GitHub](media/jenkins-java-quickstart/fork_github_repo.png)
1. Selecteer in de webconsole Jenkins **Nieuw Item**, een naam geven **MyJavaApp**, selecteer **Freestyle project**, selecteer daarna **OK**.   
    ![Nieuw Jenkins Freestyle-Project](media/jenkins-java-quickstart/jenkins_freestyle.png)
2. Onder de **algemene** sectie **GitHub** project en voer de URL van uw opslagplaats Gevorkte zoals https://github.com/raisa/gs-spring-boot-docker
3. Onder de **Source code management** sectie **Git**, Voer uw Gevorkte opslagplaats `.git` URL, zoals https://github.com/raisa/gs-spring-boot-docker.git
4. Onder de **bouwen Triggers** sectie **GitHub haakje trigger voor GITscm polling**.
5. Onder de **bouwen** sectie **toevoegen build stap** en kies **aanroepen op het hoogste niveau Maven doelen**. Voer `package` in de **doelstellingen** veld.
6. Selecteer **Opslaan**. U kunt uw taak testen door het selecteren van **nu bouwen** van de projectpagina.

## <a name="configure-azure-app-service"></a>Azure App Service configureren 

1. Met de Azure CLI of [Cloud Shell](/azure/cloud-shell/overview), maak een nieuwe [Web-App op Linux](/azure/app-service/containers/app-service-linux-intro). De naam van de web-app in deze zelfstudie is `myJavaApp`, maar u moet een unieke naam voor uw eigen app gebruiken.
   
    ```azurecli-interactive
    az group create --name myResourceGroupJenkins --location westus
    az appservice plan create --is-linux --name myLinuxAppServicePlan --resource-group myResourceGroupJenkins 
    az webapp create --name myJavaApp --resource-group myResourceGroupJenkins --plan myLinuxAppServicePlan --runtime "java|1.8|Tomcat|8.5"
    ```

2. Maak een [Azure Container register](/azure/container-registry/container-registry-intro) de Docker-afbeeldingen gebouwd door Jenkins op te slaan. De naam van de container-register gebruikt in deze zelfstudie is `jenkinsregistry`, maar u moet een unieke naam voor uw eigen register container gebruiken. 

    ```azurecli-interactive
    az acr create --name jenkinsregistry --resource-group myResourceGroupJenkins --sku Basic --admin-enabled
    ```
3. Configureren van de web-app om uit te voeren naar het register van de container gepusht Docker-installatiekopieën en opgeven dat de app in de container actief naar aanvragen op poort 8080 luistert.   

    ```azurecli-interactive
    az webapp config container set -c jenkinsregistry/webapp --resource-group myResourceGroupJenkins --name myJavaApp
    az webapp config appsettings set --resource-group myResourceGroupJenkins --name myJavaApp --settings PORT=8080
    ```

## <a name="configure-the-azure-app-service-jenkins-plug-in"></a>De invoegtoepassing van Azure App Service Jenkins configureren

1. Selecteer in de webconsole Jenkins de **MyJavaApp** taak hebt gemaakt en selecteer vervolgens **configureren** op de linkerkant van de pagina.
2. Schuif omlaag naar **na build acties**, selecteer daarna **na build actie toevoegen** en kies **een Azure-Web-App publiceren**.
3. Onder **Azure profielconfiguratie**, selecteer **toevoegen** naast **Azure-referenties** en kies **Jenkins**.
4. In de **toevoegen referenties** dialoogvenster Selecteer **Microsoft Azure Service-Principal** van de **soort** vervolgkeuzelijst.
5. Een Active Directory-Service-principal maken met Azure CLI of [Cloud Shell](/azure/cloud-shell/overview).
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
6. Voer de referenties van de service-principal in de **referenties toevoegen** dialoogvenster. Als u uw Azure-abonnement-ID niet weet, kunt u deze kunt opvragen met CLI:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    ![Azure Service-Principal configureren](media/jenkins-java-quickstart/azure_service_principal.png)
6. Controleer of de service-principal verifieert met Azure met het selecteren van **Controleer of Service-Principal**. 
7. Selecteer **toevoegen** naar de referenties op te slaan.
8. Selecteer de service principal referentie die u zojuist hebt toegevoegd uit de **Azure-referenties** vervolgkeuzelijst wanneer u terug naar bent de **een Azure-Web-App publiceren** configuratie.
9. In **App-configuratie**, kiest u de resourcegroep en web-app-naam in de vervolgkeuzelijst.
10. Selecteer de **publiceren via de Docker** keuzerondje.
11. Voer `complete/Dockerfile` voor **Dockerfile pad**.
12. Voer `https://jenkinsregistry.azurecr.io` in de **Docker register URL** veld.
13. Selecteer **toevoegen** naast **register referenties**. 
14. Voer de gebruikersnaam van de beheerder voor de Azure-Container-registersleutel die u hebt gemaakt voor de **gebruikersnaam**.
15. Voer het wachtwoord voor het register van de Azure-Container in de **wachtwoord** veld. U kunt uw gebruikersnaam en wachtwoord van de Azure-portal of via de volgende opdracht in de CLI krijgen:

    ```azurecli-interactive
    az acr credential show -n jenkinsregistry
    ```
    ![De referenties van de container register toevoegen](media/jenkins-java-quickstart/enter_acr_credentials.png)
15. Selecteer **toevoegen** om op te slaan van de referentie.
16. Selecteer de zojuist gemaakte referentie van de **register referenties** omlaag in de **App-configuratie** van het deelvenster voor de **een Azure-Web-App publiceren**. De voltooide na build-actie moet eruitzien als in de volgende afbeelding:   
    ![Post build actie configuratie voor het implementeren van Azure App Service](media/jenkins-java-quickstart/appservice_plugin_configuration.png)
17. Selecteer **opslaan** om op te slaan van de taakconfiguratie van de.

## <a name="deploy-the-app-from-github"></a>Implementeer de app vanuit GitHub

1. Selecteer in het project Jenkins **nu bouwen** de voorbeeld-app implementeren naar Azure.
2. Zodra de build is voltooid, wordt uw app live in Azure in de publicatie-URL, bijvoorbeeld http://myjavaapp.azurewebsites.net.   
   ![Uw geïmplementeerde app weergeven in Azure](media/jenkins-java-quickstart/hello_docker_world_unedited.png)

## <a name="push-changes-and-redeploy"></a>Push wijzigingen en implementeren

1. Bladeren op het web van uw fork Github `complete/src/main/java/Hello/Application.java`. Selecteer de **dit bestand bewerken** koppeling van de rechterkant van de GitHub-interface.
2. Controleer de volgende wijziging in de `home()` methode en de wijziging in de opslagplaats hoofdvertakking doorvoeren.
   
    ```java
    return "Hello Docker World on Azure";
    ```
3. Een nieuwe build wordt gestart in de Jenkins, geactiveerd door het nieuwe doorvoeren op de `master` vertakking van de opslagplaats. Nadat deze is voltooid, opnieuw laden uw app in Azure.     
      ![Uw geïmplementeerde app weergeven in Azure](media/jenkins-java-quickstart/hello_docker_world.png)
  
## <a name="next-steps"></a>Volgende stappen

- [Gebruik Azure VM's, zoals agents bouwen](/azure/jenkins/jenkins-azure-vm-agents)
- [Beheren van resources in de taken en pijplijnen met de Azure CLI](/azure/jenkins/execute-cli-jenkins-pipeline)
 
