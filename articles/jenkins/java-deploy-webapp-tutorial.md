---
title: Jenkins gebruiken voor het implementeren van uw web-apps naar Azure
description: Stel met behulp van Jenkins en Docker continue integratie van GitHub in Azure App Service voor uw Java-web-apps in.
ms.service: jenkins
keywords: jenkins, azure, devops, app-service, continue integratie, ci, continue implementatie, cd
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 07/31/2018
ms.openlocfilehash: b1af82060d316a18cd6427f70695ca4fa982064d
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106794"
---
# <a name="set-up-continuous-integration-and-deployment-to-azure-app-service-with-jenkins"></a>Continue integratie en implementatie naar Azure App Service met Jenkins instellen

In deze zelfstudie wordt met behulp van Jenkins continue integratie en implementatie (CI/CD) ingesteld voor een voorbeeld-Java-web-app die is ontwikkeld met het [Spring Boot](http://projects.spring.io/spring-boot/)-framework, voor integratie met en implementatie naar de [Azure App Service-web-app op Linux](/azure/app-service/containers/app-service-linux-intro).

In deze zelfstudie gaat u de volgende taken uitvoeren:

> [!div class="checklist"]
> * Installeer de Jenkins-invoegtoepassingen die nodig zijn om een app te implementeren naar Azure App Service.
> * Definieer een Jenkins-taak zodat Docker-installatiekopieën worden gemaakt vanuit een GitHub-opslagplaats wanneer een nieuwe doorvoering wordt gepusht.
> * Definieer een nieuwe Azure-web-app voor Linux en configureer deze voor het implementeren van Docker-installatiekopieën die naar het Azure Container-register zijn gepusht.
> * Configureer de Jenkins-invoegtoepassing voor Azure App Service.
> * Implementeer De voorbeeld-app naar Azure App Service via een handmatige build.
> * Activeer een Jenkins-build en werk de web-app bij door wijzigingen naar GitHub te pushen.

## <a name="before-you-begin"></a>Voordat u begint

Voor deze zelfstudie hebt u het volgende nodig:

* [Jenkins](https://jenkins.io/) waarvoor de hulpprogramma's JDK en Maven zijn geconfigureerd. Als u geen Jenkins-systeem hebt, maakt u er nu een in Azure met behulp van de [Jenkins-oplossingssjabloon](/azure/jenkins/install-jenkins-solution-template).
* Een [GitHub](https://github.com)-account.
* [Azure CLI 2.0](/cli/azure), die via een lokale opdrachtregel of in de [Azure Cloud Shell](/azure/cloud-shell/overview) wordt uitgevoerd.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-jenkins-plug-ins"></a>Jenkins-invoegtoepassingen configureren

1. Open een webbrowser, ga naar uw Jenkins-webconsole en selecteer **Jenkins beheren** in het menu links, selecteer vervolgens **Invoegtoepassingen beheren**.
2. Selecteer het tabblad **Beschikbaar**.
3. Zoek naar het selectievakje naast de volgende invoegtoepassingen en schakel het in:   

    - [Invoegtoepassing Azure App Service](https://plugins.jenkins.io/azure-app-service)
    - [Invoegtoepassing GitHub Branch Source](https://plugins.jenkins.io/github-branch-source)

    Als de invoegtoepassingen niet worden weergegeven, kunt u controleren of ze niet al zijn geïnstalleerd door het tabblad **Geïnstalleerd** te bekijken.

1. Selecteer **Nu downloaden en installeren na opnieuw opstarten** om de invoegtoepassingen in uw Jenkins-configuratie in te schakelen.

## <a name="configure-github-and-jenkins"></a>Docker, GitHub en Jenkins configureren

Stel Jenkins in voor het ontvangen van [GitHub-webhooks](https://developer.github.com/webhooks/) steeds als er nieuwe doorvoeringen worden gepusht naar een opslagplaats in uw account.

1. Selecteer **Jenkins beheren** en vervolgens **Systeem beheren**. In de **GitHub**-sectie controleert u of **Hooks beheren**  is geselecteerd en vervolgens selecteert u **Aanvullende GitHub-acties beheren** en kiest u **Aanmeldingsgegevens en wachtwoord naar token omzetten**.
2. Selecteer het keuzerondje **Van gebruikersnaam en wachtwoord** en voer uw GitHub-gebruikersnaam en wachtwoord in. Selecteer **Tokenreferenties maken** om een nieuw [persoonlijk toegangstoken van GitHub ](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) te maken.   
   ![Persoonlijk toegangstoken van GitHub maken van gebruikersnaam en wachtwoord](media/jenkins-java-quickstart/create_github_credentials.png)
3.  Selecteer het zojuist gemaakte token in de vervolgkeuzelijst **Referenties** in de GitHub-serversconfiguratie. Selecteer **Verbinding testen** om te controleren of de verificatie werkt.   
   ![Controleer of de verbinding met GitHub tot stand is gekomen nadat het persoonlijk toegangstoken is geconfigureerd](media/jenkins-java-quickstart/verify_github_connection.png)

> [!NOTE]
> Als voor uw GitHub-account verificatie met twee factoren is ingeschakeld, maakt u het token op GitHub en configureert u Jenkins om het gebruiken. Zie de documentatie over de [Jenkins GitHub-invoegtoepassing](https://wiki.jenkins.io/display/JENKINS/Github+Plugin) voor meer informatie.

## <a name="fork-the-sample-repo-and-create-a-jenkins-job"></a>De voorbeeldopslagplaats splitsen en een Jenkins-taak maken 

1. Open de [opslagplaats voor de Spring Boot-voorbeeldtoepassing](https://github.com/spring-guides/gs-spring-boot-docker), splits deze en koppel de gesplitste opslagplaats aan uw eigen GitHub-account **Splitsen** in de rechterbovenhoek.   
    ![Splitsen vanuit GitHub](media/jenkins-java-quickstart/fork_github_repo.png)
1. Selecteer in de Jenkins-webconsole **Nieuw Item**, geef dit de naam **MyJavaApp**, selecteer **Freestyle-project** en selecteer vervolgens **OK**.   
    ![Nieuw Jenkins-freestyle-project maken](media/jenkins-java-quickstart/jenkins_freestyle.png)
2. Selecteer onder de sectie **Algemeen** de optie **GitHub-project** en voer de URL van uw gesplitste opslagplaats in, zoals https://github.com/raisa/gs-spring-boot-docker.
3. Selecteer onder de sectie **Broncodebeheer** de optie **Git** en voer de URL van uw gesplitste opslagplaats `.git` in, zoals https://github.com/raisa/gs-spring-boot-docker.git.
4. Onder de sectie **Triggers compileren** selecteert u **GitHub hook-trigger voor GITscm-polling**.
5. In het tabblad **Maken** selecteert u **Stap voor maken toevoegen** en kiest u vervolgens **Maven-targets van het hoogste niveau aanroepen**. Voer `package` in het veld **Doelstellingen** in.
6. Selecteer **Opslaan**. U kunt uw taak testen door op de projectpagina de optie **Nu maken** te selecteren.

## <a name="configure-azure-app-service"></a>Azure App Service configureren 

1. Met behulp van Azure CLI of [Cloud Shell](/azure/cloud-shell/overview) een nieuwe [web-app op Linux](/azure/app-service/containers/app-service-linux-intro) maken De naam van de web-app in deze zelfstudie is `myJavaApp`, maar u moet een unieke naam voor uw eigen app gebruiken.
   
    ```azurecli-interactive
    az group create --name myResourceGroupJenkins --location westus
    az appservice plan create --is-linux --name myLinuxAppServicePlan --resource-group myResourceGroupJenkins 
    az webapp create --name myJavaApp --resource-group myResourceGroupJenkins --plan myLinuxAppServicePlan --runtime "java|1.8|Tomcat|8.5"
    ```

2. Maak een [Azure Container Registry](/azure/container-registry/container-registry-intro) waarin u de Docker-installatiekopieën opslaat die u met Jenkins hebt gemaakt. De naam van het containerregister die in deze zelfstudie wordt gebruikt is `jenkinsregistry`, maar u moet een unieke naam voor uw eigen containerregister gebruiken. 

    ```azurecli-interactive
    az acr create --name jenkinsregistry --resource-group myResourceGroupJenkins --sku Basic --admin-enabled
    ```
3. Configureer de web-app voor het uitvoeren van Docker-installatiekopieën die naar het containerregister zijn gepusht en geef op dat de app die in de container wordt uitgevoerd, naar aanvragen op poort 8080 luistert.   

    ```azurecli-interactive
    az webapp config container set -c jenkinsregistry/webapp --resource-group myResourceGroupJenkins --name myJavaApp
    az webapp config appsettings set --resource-group myResourceGroupJenkins --name myJavaApp --settings PORT=8080
    ```

## <a name="configure-the-azure-app-service-jenkins-plug-in"></a>De Jenkins-invoegtoepassing voor Azure App Service configureren

1. Selecteer in de Jenkins-webconsole de taak **MyJavaApp** die u hebt gemaakt en selecteer vervolgens **Configureren** links op de pagina.
2. Schuif omlaag naar **Acties na maken** en selecteer **Actie na maken toevoegen** en kies **Een Azure-web-app publiceren**.
3. Selecteer bij **Configuratie Azure-profiel** de optie **Toevoegen** naast **Azure-referenties** en kies **Jenkins**.
4. In het dialoogvenster **Referenties toevoegen** selecteert u **Microsoft Azure-service-principal** uit de vervolgkeuzelijst **Soort**.
5. Maak een Active Directory-service-principal vanuit de Azure CLI of [Cloud Shell](/azure/cloud-shell/overview).
    
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
6. Voer de referenties vanuit de service-principal in het dialoogvenster **Referenties toevoegen** in. Als u uw Azure-abonnements-id niet weet, kunt u deze opvragen via de CLI:
     
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

    ![Azure Service Principal configureren](media/jenkins-java-quickstart/azure_service_principal.png)
6. Controleer of de service-principal wordt geverifieerd bij Azure door **Service-principal verifiëren** te selecteren. 
7. Selecteer **Toevoegen** om de referenties op te slaan.
8. Selecteer de service-principalreferentie die u zojuist hebt toegevoegd uit de vervolgkeuzelijst **Azure-referenties** als u weer terug bent bij de configuratie **Een Azure-app publiceren**.
9. Kies bij **App-configuratie** de resourcegroep en web-appnaam in de vervolgkeuzelijst.
10. Selecteer het keuzerondje **Publiceren via Docker**.
11. Voer `complete/Dockerfile` in bij **Pad naar Dockerfile**.
12. Typ `https://jenkinsregistry.azurecr.io` in het veld **Docker-register-URL**.
13. Selecteer **Toevoegen** naast **Registerreferenties**. 
14. Voer de gebruikersnaam in van de beheerder voor het Azure-containerregister dat u hebt gemaakt voor de **Gebruikersnaam**.
15. Geef het wachtwoord voor het Azure-containerregister op in het veld **Wachtwoord**. U kunt uw gebruikersnaam en wachtwoord verkrijgen via de Azure-portal of via de volgende CLI-opdracht:

    ```azurecli-interactive
    az acr credential show -n jenkinsregistry
    ```
    ![Uw referenties voor het containerregister toevoegen](media/jenkins-java-quickstart/enter_acr_credentials.png)
15. Selecteer **Toevoegen** om de referentie op te slaan.
16. Selecteer de zojuist gemaakte referentie in de vervolgkeuzelijst **Registerreferenties** in het deelvenster **App-configuratie** voor **Een Azure-web-app publiceren**. De voltooide actie na het maken moet er uitzien zoals op de volgende afbeelding:   
    ![Actie na maken voor configureren voor Azure App Service-implementatie](media/jenkins-java-quickstart/appservice_plugin_configuration.png)
17. Selecteer **opslaan** om de taakdefinitie op te slaan.

## <a name="deploy-the-app-from-github"></a>De app vanuit GitHub implementeren

1. Selecteer in het Jenkins-project de optie **Nu maken** om de voorbeeld-app naar Azure te implementeren.
2. Nadat de maakactie is voltooid, is uw app live te vinden in Azure via de publicatie-URL, bijvoorbeeld http://myjavaapp.azurewebsites.net.   
   ![Uw geïmplementeerde app op Azure weergeven](media/jenkins-java-quickstart/hello_docker_world_unedited.png)

## <a name="push-changes-and-redeploy"></a>Wijzigingen pushen en opnieuw implementeren

1. Bladeren op het web vanuit in uw Github-fork naar `complete/src/main/java/Hello/Application.java`. Selecteer de koppeling **Dit bestand bewerken** rechts in de GitHub-interface.
2. Breng de volgende wijziging aan in de methode `home()` en voer deze door in de hoofdvertakking van de opslagplaats.
   
    ```java
    return "Hello Docker World on Azure";
    ```
3. Er wordt een nieuwe maakactie gestart in Jenkins, geactiveerd door de nieuwe doorvoering in de `master`vertakking van de opslagplaats. Zodra deze actie is voltooid, laadt u uw app opnieuw in Azure.     
      ![Uw geïmplementeerde app weergeven op Azure](media/jenkins-java-quickstart/hello_docker_world.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Problemen met de Jenkins-invoegtoepassing oplossen

Als u problemen ondervindt met de Jenkins-invoegtoepassingen, kunt u in [Jenkins JIRA](https://issues.jenkins-ci.org/) een ticket openen voor het specifieke onderdeel.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure-VM's als bouwagenten gebruiken](/azure/jenkins/jenkins-azure-vm-agents)