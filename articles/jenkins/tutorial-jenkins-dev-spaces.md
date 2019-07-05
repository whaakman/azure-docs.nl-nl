---
title: Gebruik van de Azure Dev spaties-invoegtoepassing voor Jenkins met Azure Kubenetes-Service
description: Informatie over het gebruik van de invoegtoepassing Azure Dev spaties in een pijplijn voor continue integratie.
author: tomarchermsft
ms.author: tarcher
ms.service: jenkins
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/18/2019
ms.openlocfilehash: 8c47b8caf2d289ed17647b8003cc702156f3cddb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592079"
---
<!-- GMinchAQ, 06/18/19 -->

# <a name="tutorial-using-the-azure-dev-spaces-plugin-for-jenkins-with-azure-kubenetes-service"></a>Zelfstudie: Gebruik van de Azure Dev spaties-invoegtoepassing voor Jenkins met Azure Kubenetes-Service 

Azure Dev opslagruimten kunt u om te testen en ontwikkelen van uw microservice-toepassing die wordt uitgevoerd in Azure Kubernetes Service (AKS) zonder de noodzaak om te repliceren of gesimuleerde afhankelijkheden van een iteratief. De Azure Dev spaties-invoegtoepassing voor Jenkins helpt u bij gebruik van Dev spaties in uw continue integratie en levering (CI/CD)-pijplijn.

In deze zelfstudie maakt ook gebruik van Azure Container Registry (ACR). ACR afbeeldingen opslaat en een ACR-taak bouwt Docker en Helm-artefacten. Met ACR en ACR-taak voor het genereren van het artefact hoeven zich niet voor u extra software, zoals Docker, te installeren op uw Jenkins-server. 

In deze zelfstudie voert u de volgende taken uit:

> [!div class="checklist"]
> * Een Azure Dev spaties ingeschakelde AKS-cluster maken
> * Een AKS-toepassing meerdere services implementeren
> * Uw Jenkins-server voorbereiden
> * De invoegtoepassing Azure Dev spaties in een pijplijn met Jenkins gebruiken om een voorbeeld van code hoeft te wijzigen voordat u ze samen te voegen in het project

In deze zelfstudie wordt ervan uitgegaan dat tussenliggende kennis van de belangrijkste Azure-services, AKS, ACR, Azure Dev spaties, Jenkins [pijplijnen](https://jenkins.io/doc/book/pipeline/) en invoegtoepassingen, en GitHub. Het zou mooi met ondersteunende hulpprogramma's zoals kubectl en Helm is handig.

## <a name="prerequisites"></a>Vereisten

* Een Azure-account. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

* Een GitHub-account. Als u geen een GitHub-account hebt, maakt u een [gratis account](https://github.com/) voordat u begint.

* [Visual Studio Code](https://code.visualstudio.com/download) met de [Azure Dev spaties](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) -extensie geïnstalleerd.

* [Azure CLI geïnstalleerd](/cli/azure/install-azure-cli?view=azure-cli-latest), versie 2.0.43 of hoger.

* Een Jenkins-masterserver. Als u nog een Jenkins-master hebt, implementeert u [Jenkins](https://aka.ms/jenkins-on-azure) op Azure met de volgende stappen in deze [snelstartgids](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* De Jenkins-server moet zowel Helm en kubectl geïnstalleerd en beschikbaar zijn voor de Jenkins-account hebben, zoals verderop in deze zelfstudie wordt beschreven.

* VS Code de Terminal van VS Code of WSL en Bash. 


## <a name="create-azure-resources"></a>Azure-resources maken

In deze sectie maakt maken u Azure-resources:

* Een resourcegroep bevat al de Azure-resources voor deze zelfstudie.
* Een [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) (AKS)-cluster.
* Een [Azure-containerregister](https://docs.microsoft.com/azure/container-registry/) (ACR) om te bouwen (met behulp van de ACR-taken) en opslaan van Docker-installatiekopieën.

1. Maak een resourcegroep.

    ```bash
    az group create --name MyResourceGroup --location westus2
    ```

2. Een AKS-cluster maken. Maken van het AKS-cluster in een [regio die ondersteuning biedt voor opslagruimten Dev](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams).

    ```bash
    az aks create --resource-group MyResourceGroup --name MyAKS --location westus2 --kubernetes-version 1.11.9 --enable-addons http_application_routing --generate-ssh-keys --node-count 1 --node-vm-size Standard_D1_v2
    ```

3. Configureer AKS gebruik Dev spaties.

    ```bash
    az aks use-dev-spaces --resource-group MyResourceGroup --name MyAKS
    ```
    Deze stap installeert de `azds` CLI-extensie.

4. Een containerregister maken.

    ```bash
    az acr create -n MyACR -g MyResourceGroup --sku Basic --admin-enabled true
    ```

## <a name="deploy-sample-apps-to-the-aks-cluster"></a>Voorbeeld-apps implementeren op het AKS-cluster

In deze sectie instellen van een dev-ruimte en implementeert u een voorbeeld van toepassing op het AKS-cluster dat u in de laatste sectie hebt gemaakt. De toepassing bestaat uit twee delen: *webfrontend* en *mywebapi*. Beide onderdelen worden geïmplementeerd in een dev-ruimte. Verderop in deze zelfstudie hebt u een pull-aanvraag voor mywebapi voor het activeren van de CI-pijplijn in Jenkins indienen.

Zie voor meer informatie over het gebruik van Azure Dev spaties en de ontwikkeling van meerdere services met Azure Dev spaties [aan de slag met Azure Dev ruimten met behulp van Java](https://docs.microsoft.com/azure/dev-spaces/get-started-java), en [meerdere services ontwikkelen met Azure Dev spaties](https://docs.microsoft.com/azure/dev-spaces/multi-service-java). Deze zelfstudies vindt u aanvullende achtergrondinformatie hier niet zijn opgenomen.

1. Download de https://github.com/Azure/dev-spaces opslagplaats van GitHub.

2. Open de `samples/java/getting-started/webfrontend` map in VS Code. (U kunt eventuele standaardprompts negeren om foutopsporingsassets toe te voegen of om het project te herstellen.)

3. Update `/src/main/java/com/ms/sample/webfrontend/Application.java` zodat het eruitziet als het volgende:

    ```java
    package com.ms.sample.webfrontend;

    import java.io.*;
    import java.net.*;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.web.bind.annotation.*;

    @SpringBootApplication
    @RestController
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }

        @RequestMapping(value = "/greeting", produces = "text/plain")
        public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
            URLConnection conn = new URL("http://mywebapi/").openConnection();
            conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
            try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
            {
                return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
            }
        }
    }
    ```

4. Klik op **weergave** vervolgens **Terminal** de geïntegreerde Terminal te openen in VS Code.

5. Voer de `azds prep` opdracht voor het voorbereiden van uw toepassing om uit te voeren in een dev-ruimte. Met deze opdracht moet worden uitgevoerd vanaf `dev-spaces/samples/java/getting-started/webfrontend` correct voorbereiden van uw toepassing:

    ```bash
    azds prep --public
    ```

    De Dev spaties CLI `azds prep` Docker en Kubernetes activa met standaardinstellingen wordt nu gegenereerd. Deze bestanden blijven behouden gedurende de levensduur van het project en ze kunnen worden aangepast:

    * `./Dockerfile` en `./Dockerfile.develop` beschrijven van de app container-installatiekopie, en hoe de broncode wordt opgebouwd en wordt uitgevoerd in de container.
    * Met een [Helm-grafiek](https://helm.sh/docs/developing_charts/) die zich onder `./charts/webfrontend` bevindt, wordt beschreven hoe de container in Kubernetes moet worden geïmplementeerd.
    * `./azds.yaml` is het configuratiebestand Azure Dev spaties.

    Zie voor meer informatie, [hoe Azure Dev spaties werkt en is geconfigureerd](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works).

6. Ontwikkelen en uitvoeren van de toepassing in AKS met behulp van de `azds up` opdracht:

    ```bash
    azds up
    ```
    <a name="test_endpoint"></a>Scannen van de uitvoer van de console voor meer informatie over de URL die is gemaakt door de `up` opdracht. Het zal in deze vorm te zien zijn:

    ```bash
    (pending registration) Service 'webfrontend' port 'http' will be available at '<url>'
    Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
    ```
    Deze URL in een browservenster geopend en ziet u de web-app. Terwijl de container wordt uitgevoerd, wordt `stdout`- en `stderr`-uitvoer naar het terminalvenster gestreamd.

8. Vervolgens ingesteld en implementeer *mywebapi*:

    1. Wijzig de map naar `dev-spaces/samples/java/getting-started/mywebapi`

    2. Voer

        ```bash
        azds prep
        ```

    3. Voer

        ```bash
        azds up -d
        ```

## <a name="prepare-jenkins-server"></a>Jenkins-server voorbereiden

In deze sectie maakt voorbereiden u de Jenkins-server om uit te voeren van de voorbeeld-CI-pijplijn.

* Invoegtoepassingen installeren
* Helm en Kubernetes-CLI installeren
* Referenties toevoegen

### <a name="install-plugins"></a>Invoegtoepassingen installeren

1. Aanmelden bij uw Jenkins-server. Kies **Jenkins beheren > invoegtoepassingen beheren**.
2. Op de **beschikbaar** tabblad, selecteert u de volgende invoegtoepassingen:
    * [Azure Dev-opslagruimten](https://plugins.jenkins.io/azure-dev-spaces)
    * [Azure Container Registry-taken](https://plugins.jenkins.io/azure-container-registry-tasks)
    * [Omgeving Injector](https://plugins.jenkins.io/envinject)
    * [GitHub-integratie](https://plugins.jenkins.io/github-pullrequest)

    Als deze invoegtoepassingen worden niet in de lijst weergegeven, controleert u de **geïnstalleerde** tabblad om te zien als ze al zijn geïnstalleerd.

3. Als u wilt installeren van de invoegtoepassingen **nu downloaden en installeren na opnieuw opstarten**.

4. Start opnieuw op uw Jenkins-server om de installatie te voltooien.

### <a name="install-helm-and-kubectl"></a>Helm en kubectl installeren

De voorbeeldpijplijn maakt gebruik van Helm en kubectl om te implementeren op de dev-ruimte. Wanneer u Jenkins is geïnstalleerd, maakt een beheerdersaccount met de naam *jenkins*. Zowel Helm en kubectl moeten toegankelijk zijn voor de jenkins-gebruiker.

1. Een SSH-verbinding maken met de Jenkins-master. 

2. Schakel over naar de `jenkins` gebruiker:
    ```bash
    sudo su jenkins
    ```

3. Installeer de Helm CLI. Zie voor meer informatie, [Helm installeren](https://helm.sh/docs/using_helm/#installing-helm).

4. Kubectl installeren. Zie voor meer informatie, [ **az acs kubernetes install-cli**](https://helm.sh/docs/using_helm/#installing-helm).

### <a name="add-credentials-to-jenkins"></a>Referenties toevoegen met Jenkins

1. Jenkins moet een Azure service-principal voor de verificatie en toegang tot Azure-resources. Voor het maken van de service-principal, verwijzen naar de [service-principal maken](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) sectie in de implementeren in Azure App Service-zelfstudie. Bewaar een kopie van de uitvoer van `create-for-rbac` omdat u nodig die gegevens hebt voor het voltooien van de volgende stap. De uitvoer ziet er ongeveer als volgt:

    ```json
    {
      "appId": "f4150da1-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "displayName": "xxxxxxxjenkinssp",
      "name": "http://xxxxxxxjenkinssp",
      "password": "f6e4d839-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "tenant": "72f988bf--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. Voeg een *Microsoft Azure Service-Principal* referentietype in Jenkins, met behulp van de gegevens uit de vorige stap in de service-principal. De namen in de onderstaande schermafbeelding komen overeen met de uitvoer van `create-for-rbac`.

    De **ID** veld is de naam van de Jenkins-referentie voor uw service-principal. Het voorbeeld wordt de waarde van `displayName` (in dit geval `xxxxxxxjenkinssp`), maar u kunt de tekst die u wilt gebruiken. De referentienaam van deze is de waarde voor de omgevingsvariabele AZURE_CRED_ID in de volgende sectie.

    ![Referenties voor service-principal toevoegen aan Jenkins](media/tutorial-jenkins-dev-spaces/add-service-principal-credentials.png)

    De **beschrijving** is optioneel. Zie voor meer instructies, [service-principal toevoegen voor Jenkins](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#add-service-principal-to-jenkins) sectie in de implementeren in Azure App Service-zelfstudie. 



3. Als u wilt weergeven van uw ACR-referenties, moet u deze opdracht uitvoeren:

    ```bash
    az acr credential show -n <yourRegistryName>
    ```

    Maak een kopie van de JSON-uitvoer, die ziet er ongeveer als volgt:

    ```json
    {
      "passwords": [
        {
          "name": "password",
          "value": "vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz"
        },
        {
          "name": "password2",
          "value": "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
        }
      ],
      "username": "acr01"
    }
    ```

4. Voeg een *gebruikersnaam met wachtwoord* referentietype in Jenkins. De **gebruikersnaam** is de gebruikersnaam van de laatste stap in dit voorbeeld `acr01`. De **wachtwoord** is de waarde voor het eerste wachtwoord, in dit voorbeeld `vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz`. De **ID** van deze referentie is de waarde van ACR_CRED_ID.

5. Instellen van een AKS-referentie. Voeg een *configuratie van het Kubernetes (kubeconfig)* referentietype in Jenkins (Gebruik de optie 'Rechtstreeks Enter'). Als u de referenties voor toegang voor uw AKS-cluster, moet u de volgende opdracht uitvoeren:

    ```cmd
    az aks get-credentials -g MyResourceGroup -n <yourAKSName> -f -
    ```

   De **ID** deze referentie is de waarde van KUBE_CONFIG_ID in de volgende sectie.

## <a name="create-a-pipeline"></a>Een pijplijn maken

Het scenario dat is geselecteerd voor de pijplijn voorbeeld is gebaseerd op een echte-patroon: Een pull-aanvraag triggers een CI-pijplijn die bouwt en implementeert dan de door u voorgestelde wijzigingen aan een Azure dev-ruimte voor het testen en controleren. Afhankelijk van het resultaat van de evaluatie, worden de wijzigingen samengevoegd en geïmplementeerd in AKS of verwijderd. Ten slotte is de dev-ruimte verwijderd.

De configuratie van de Jenkins-pijplijn en Jenkinsfile definieert de fasen in de CI-pijplijn. Deze stroomdiagram ziet u de pijplijnfasen en certificaattoewijzing gedefinieerd door de Jenkinsfile:

![Stroom van Jenkins-pijplijn](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-flow.png)

1. Download een gewijzigde versie van de *mywebapi* project in https://github.com/azure-devops/mywebapi. Dit project bevat verschillende bestanden die nodig zijn om een pijplijn te maken met inbegrip van de *Jenkinsfile*, *docker-bestanden*, en Helm-diagram.

2. Meld u aan bij Jenkins. Selecteer in het menu aan de linkerkant, **Item toevoegen**.

3. Selecteer **pijplijn**, en voer een naam in de **Voer een itemnaam in** vak. Selecteer **OK**, en vervolgens het Configuratiescherm van de pijplijn voor het automatisch wordt geopend.

4. Op de **algemene** tabblad controle **voorbereiden van een omgeving voor de uitvoerbewerking**. 

5. Controleer **Jenkins omgevingsvariabelen houden** en **houden van de variabelen voor de Jenkins-Build**.

6. In de **eigenschappen inhoud** voert u de volgende omgevingsvariabelen:

    ```
    AZURE_CRED_ID=[your credential ID of service principal]
    RES_GROUP=[your resource group of the function app]
    ACR_RES_GROUP=[your ACR resource group]
    ACR_NAME=[your ACR name]
    ACR_REGISTRY=[your ACR registry url, without http schema]
    ACR_CRED_ID=[your credential id of your ACR account]
    AKS_RES_GROUP=[your AKS resource group]
    AKS_NAME=[your AKS name]
    IMAGE_NAME=[name of Docker image you will push to ACR, without registry prefix]
    KUBE_CONFIG_ID=[your kubeconfig id]
    PARENT_DEV_SPACE=[shared dev space name]
    TEST_ENDPOINT=[your web frontend end point for testing. Should be webfrontend.XXXXXXXXXXXXXXXXXXX.xxxxxx.aksapp.io]
    ```

    Met behulp van de voorbeeldwaarden opgegeven in de voorgaande secties, ziet de lijst met omgevingsvariabelen er ongeveer als volgt:

    ![Omgevingsvariabelen voor Jenkins-pijplijn](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-environment.png)

7. Kies **Pipeline-script uit SCM** in **pijplijn > definitie**.
8. In **SCM**, kiest u **Git** en voer de URL van uw opslagplaats.
9. In **vertakking aanduiding**, voer `refs/remotes/origin/${GITHUB_PR_SOURCE_BRANCH}`.
10. Vul in de SCM-opslagplaats-URL en het script pad 'Jenkinsfile'.
11. **Lichtgewicht afhandeling** moeten worden gecontroleerd.

## <a name="create-a-pull-request-to-trigger-the-pipeline"></a>Maak een pull-aanvraag voor het activeren van de pijplijn

Als u stap 3 in deze sectie, moet u deel uit van de Jenkinsfile opmerking, anders krijgt u een 404-fout wanneer u probeert om de nieuwe en oude versies naast elkaar weer te geven. Standaard, als u kiest om de pull-aanvraag, samen te voegen worden de vorige gedeelde versie van mywebapi verwijderd en vervangen door de nieuwe versie. Breng de volgende wijziging aan de Jenkinsfile voordat u stap 1:

```Groovy
    if (userInput == true) {
        stage('deploy') {
            // Apply the deployment to shared namespace in AKS using acsDeploy, Helm or kubectl   
        }
        
        stage('Verify') {
            // verify the staging environment is working properly
        }
        
        /* Comment the cleanup stage to allow side by side comparison with the new child dev space

        stage('cleanup') {
            devSpacesCleanup aksName: env.AKS_NAME, 
                azureCredentialsId: env.AZURE_CRED_ID, 
                devSpaceName: devSpaceNamespace, 
                kubeConfigId: env.KUBE_CONFIG_ID, 
                resourceGroupName: env.AKS_RES_GROUP,
                helmReleaseName: releaseName
        }
        */

    } else {
        // Send a notification
    }
```

1. Een wijziging aanbrengt `mywebapi/src/main/java/com/ms/sample/mywebapi/Application.java`en maak vervolgens een pull-aanvraag. Bijvoorbeeld:

    ```java
    public String index() {
        return "Hello from mywebapi in my private dev space";
    }
    ```

2. Meld u aan bij Jenkins en selecteer de pijplijnnaam van de en kies vervolgens **Build Now**. 

    U kunt ook instellen van een *webhook* om automatisch de Jenkins-pijplijn. Wanneer een pull-aanvraag wordt ingevoerd, problemen GitHub een bericht met Jenkins, de pijplijn activeren. Zie voor meer informatie over het instellen van een webhook [verbinding maken met Jenkins naar GitHub](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service.md#connect-jenkins-to-github).

3. Wijzigingen in de huidige versie van de gedeelde vergelijken:

    1. Open uw browser en navigeer naar de gedeelde versie `https://webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. TEST_ENDPOINT bevat de URL.

    2. Een ander tabblad openen en voer vervolgens de URL van de pull-aanvraag dev-ruimte. Dit is vergelijkbaar met `https://<yourdevspacename>.s.webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. U vindt de koppeling in **bouwen geschiedenis >< build # >> Console-uitvoer** voor de Jenkins-taak. Zoeken naar de pagina voor `aksapp`, of als u wilt zien alleen de voorvoegsel, zoek naar `azdsprefix`.

 

### <a name="constructing-the-url-to-the-child-dev-space"></a>De URL naar de onderliggende dev-ruimte maken

Als u een pull-aanvraag, wordt Jenkins maakt u een onderliggende dev-ruimte op basis van gedeelde dev-ruimte van het team en voert de code van uw pull-aanvraag in die onderliggende dev-ruimte. De URL naar de onderliggende dev-ruimte heeft de vorm `http://$env.azdsprefix.<test_endpoint>`. 

**$env.azdsprefix** wordt ingesteld tijdens de pijplijnuitvoering door de invoegtoepassing Azure Dev spaties door **devSpacesCreate**:

```Groovy
stage('create dev space') {
    devSpacesCreate aksName: env.AKS_NAME,
        azureCredentialsId: env.AZURE_CRED_ID,
        kubeconfigId: env.KUBE_CONFIG_ID,
        resourceGroupName: env.AKS_RES_GROUP,
        sharedSpaceName: env.PARENT_DEV_SPACE,
        spaceName: devSpaceNamespace
}
```

De `test_endpoint` is de URL naar de webfrontend-app die u eerder hebt geïmplementeerd met behulp van `azds up`in [voorbeeld-apps implementeren op het AKS-cluster, stap 7](#test_endpoint). De waarde van `$env.TEST_ENDPOINT` is ingesteld in de configuratie van de pijplijn. 

Het volgende codefragment toont hoe de URL van de onderliggende dev-ruimte wordt gebruikt in de `smoketest` fase. De code controleert u of de onderliggende dev ruimte TEST_ENDPOINT beschikbaar is en als dit het geval is, downloadt de begroeting tekst naar de stdout:

```Groovy
stage('smoketest') {
    // CI testing against http://$env.azdsprefix.$env.TEST_ENDPOINT" 
    SLEEP_TIME = 30
    SITE_UP = false
    for (int i = 0; i < 10; i++) {
        sh "sleep ${SLEEP_TIME}"
        code = "0"
        try {
            code = sh returnStdout: true, script: "curl -sL -w '%{http_code}' 'http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting' -o /dev/null"
        } catch (Exception e){
            // ignore
        }
        if (code == "200") {
            sh "curl http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting"
            SITE_UP = true
            break
        }
    }
    if(!SITE_UP) {
        echo "The site has not been up after five minutes"
    }
}
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met behulp van de voorbeeldtoepassing, opschonen van de Azure-resources door de resourcegroep te verwijderen:

```bash
az group delete -y --no-wait -n MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u de Azure Dev spaties-invoegtoepassing voor Jenkins en de invoegtoepassing Azure Container Registry code bouwen en implementeren naar een dev-ruimte.

De volgende lijst van resources bevat meer informatie over Azure Dev spaties, ACR-taken en CI/CD met Jenkins.

Azure Dev spaties:
* [Hoe Azure Dev Spaces werkt en is geconfigureerd](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works)

ACR-taken:
* [OS- en frameworkpatching automatiseren met ACR Tasks](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview)
* [Automatische build op code doorvoeren](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview)

CI/CD met Jenkins op Azure:
* [Doorlopende Jenkins-implementatie](https://docs.microsoft.com/azure/aks/jenkins-continuous-deployment)
