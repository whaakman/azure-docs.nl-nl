---
title: Implementeren naar Azure Kubernetes Service (AKS) met behulp van Jenkins en het patroon blue/green implementatie
description: Informatie over het implementeren naar Azure Kubernetes Service (AKS) met behulp van Jenkins en het patroon blue/green implementatie.
services: app-service\web
documentationcenter: ''
author: tomarcher
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 07/23/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 384681ae0ba212b485022ac81743528f96075ec8
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716454"
---
# <a name="deploy-to-azure-kubernetes-service-aks-by-using-jenkins-and-the-bluegreen-deployment-pattern"></a>Implementeren naar Azure Kubernetes Service (AKS) met behulp van Jenkins en het patroon blue/green implementatie

Azure Kubernetes Service (AKS) beheert uw gehoste Kubernetes-omgeving, waardoor u snel en eenvoudig te implementeren en beheren van toepassingen in containers. U hoeft geen expertise in container-orchestration. AKS elimineert ook de last van lopende bewerkingen en onderhoud, inrichten, upgraden en vergroten/verkleinen resources op aanvraag. U hoeft niet offline zetten van uw toepassingen. Zie voor meer informatie over AKS de [AKS-documentatie](/azure/aks/).

Blue/green implementatie is een continue levering van Azure DevOps-patroon die is gebaseerd op een bestaande (blauw) versie live, houden terwijl (groen) een nieuwe wordt geïmplementeerd. Normaal gesproken load balancing-om de steeds grotere hoeveelheden verkeer naar de groene implementatie maakt gebruik van dit patroon. Als een incident bewaking heeft gedetecteerd, kan verkeer worden omgeleid naar de blauwe implementatie, die nog steeds wordt uitgevoerd. Zie voor meer informatie over continue levering [wat continue levering is](/azure/devops/what-is-continuous-delivery).

In deze zelfstudie leert u hoe u de volgende taken uitvoeren:

> [!div class="checklist"]
> * Meer informatie over het patroon blue/green implementatie
> * Een beheerde Kubernetes-cluster maken
> * Een voorbeeld van een script voor het configureren van een Kubernetes-cluster worden uitgevoerd
> * Handmatig configureren van een Kubernetes-cluster
> * Maken en uitvoeren van een Jenkins-taak

## <a name="prerequisites"></a>Vereisten
- [GitHub-account](https://github.com) : U moet een GitHub-account de voorbeeldopslagplaats te klonen.
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) : U de Azure CLI 2.0 gebruiken om de Kubernetes-cluster te maken.
- [Chocolatey](https://chocolatey.org): een pakketbeheerder die u gebruikt voor het installeren van kubectl.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): een opdrachtregelinterface die u gebruikt voor het uitvoeren van opdrachten op basis van Kubernetes-clusters.
- [jq](https://stedolan.github.io/jq/download/): een lichtgewicht, opdrachtregel JSON-processor.

## <a name="clone-the-sample-app-from-github"></a>Kloon de voorbeeld-app vanuit GitHub

Op de Microsoft-opslagplaats in GitHub vindt u een voorbeeld-app die laat hoe u kunt implementeren naar AKS zien met behulp van Jenkins en het patroon blauw/groen. In deze sectie maakt een fork van-opslagplaats die in uw GitHub en kloon van de app naar uw lokale systeem.

1. Blader naar de GitHub-opslagplaats voor de [todo-app-java-op-azure](https://github.com/microsoft/todo-app-java-on-azure.git) voorbeeld-app.

    ![Schermopname van voorbeeld-app op Microsoft GitHub-opslagplaats](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. De repo splitsen door te selecteren **Fork** in de rechterbovenhoek van de pagina en volg de instructies voor het splitsen van de opslagplaats in uw GitHub-account.

    ![Schermafbeelding van de GitHub-optie voor het fork](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Nadat u de repo splitsen, ziet u dat de naam van de wijzigingen in de accountnaam van uw en een opmerking geeft aan vanaf waar de opslagplaats is gesplitst (Microsoft).

    ![Schermafbeelding van de GitHub-accountnaam en de opmerking](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Selecteer **klonen of downloaden**.

    ![Schermafbeelding van de GitHub-optie voor het klonen of downloaden van een opslagplaats](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. In de **klonen met HTTPS** venster de **kopie** pictogram.

    ![Schermafbeelding van de GitHub-optie voor de kloon-URL naar het Klembord kopiëren](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Open een terminal of Git Bash-venster.

1. Wijzig de mappen in de gewenste locatie waar u de lokale kopie (gekloond) van de opslagplaats op te slaan.

1. Met behulp van de `git clone` opdracht, het klonen van de URL die u eerder hebt gekopieerd.

    ![Schermafbeelding van Git Bash git kloon-opdracht](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Druk op de Enter-toets om de kloonproces te starten.

    ![Schermafbeelding van Git Bash git kloon opdrachtresultaten](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Wijzig de mappen in de zojuist gemaakte map waarin de kloon van de bron-app.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Maken en configureren van een beheerde Kubernetes-cluster

In deze sectie maakt uitvoeren u de volgende stappen:

- De Azure CLI 2.0 gebruiken om te maken van een beheerde Kubernetes-cluster.
- Meer informatie over het instellen van een cluster met behulp van de setup-script of handmatig.
- Maak een instantie van de Azure Container Registry-service.

> [!NOTE]   
> AKS is momenteel in preview. Zie voor meer informatie over het inschakelen van de Preview-versie voor uw Azure-abonnement [Snelstartgids: een Azure Kubernetes Service (AKS)-cluster implementeren](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription).

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>De Azure CLI 2.0 gebruiken om te maken van een beheerde Kubernetes-cluster
Als u wilt maken van een beheerde Kubernetes-cluster met de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), zorg ervoor dat u van Azure CLI versie 2.0.25 gebruikmaakt of later.

1. Aanmelden bij uw Azure-account. Nadat u de volgende opdracht hebt ingevoerd, ontvangt u instructies waarin wordt uitgelegd hoe u de aanmelding is voltooid. 
    
    ```bash
    az login
    ```

1. Bij het uitvoeren van de `az login` opdracht in de vorige stap, verschijnt een lijst met alle uw Azure-abonnementen (samen met hun abonnement-id's). In deze stap stelt u de standaard Azure-abonnement. Vervang de &lt;your subscription-id > tijdelijke aanduiding door de gewenste Azure-abonnement-ID. 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Maak een resourcegroep. Vervang de &lt;uw resource-group-naam > tijdelijke aanduiding door de naam van uw nieuwe resourcegroep en vervang de &lt;uw locatie > tijdelijke aanduiding door de locatie. De opdracht `az account list-locations` weergegeven van alle Azure-locaties. Tijdens de preview AKS zijn niet alle locaties beschikbaar. Als u een locatie die is niet geldig op dit moment invoert, bevat het foutbericht de beschikbare locaties.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. De Kubernetes-cluster maken. Vervang de &lt;uw resource-group-naam > met de naam van de resourcegroep gemaakt in de vorige stap en vervang de &lt;uw kubernetes-cluster-naam > door de naam van het nieuwe cluster. (Dit proces kan enkele minuten duren).

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Instellen van het Kubernetes-cluster

U kunt een blue/green implementatie in AKS handmatig instellen of met een script dat is opgegeven in het voorbeeld eerder hebt gekloond. In deze sectie ziet u hoe u beide.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Instellen van het Kubernetes-cluster via de setup-voorbeeldscript
1. Bewerk de **deploy/aks/setup/setup.sh** bestand en vervang de volgende tijdelijke aanduidingen door de juiste waarden voor uw omgeving: 

    - **&lt;uw resource-group-naam >**
    - **&lt;uw kubernetes-cluster-naam >**
    - **&lt;uw locatie >**
    - **&lt;uw-dns-naam-achtervoegsel >**

    ![Schermafbeelding setup.sh script in bash, met verschillende tijdelijke aanduidingen die zijn gemarkeerd](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Voer het installatiescript uit.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>Handmatig een Kubernetes-cluster instellen 
1. Download de configuratie van het Kubernetes naar de profielmap van uw.

    ```bash
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Wijzig de map in de **implementeren/aks/setup** directory. 

1. Voer de volgende **kubectl** opdrachten voor het instellen van de services voor het openbare eindpunt en de eindpunten van de test.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Update voor de DNS-naam voor het publiek en test-eindpunten. Wanneer u een Kubernetes-cluster maakt, maakt u ook een [extra resourcegroep](https://github.com/Azure/AKS/issues/3), met het naamgevingspatroon van **MC_&lt;uw resource-group-naam >_ &lt; uw kubernetes-cluster-naam >_&lt;uw locatie >**.

    Ga naar het openbare IP-adressen in de resourcegroep.

    ![Schermopname van het openbare IP-adressen in de resourcegroep](./media/jenkins-aks-blue-green-deployment/publicip.png)

    Voor elk van de services, vindt u het externe IP-adres door het uitvoeren van de volgende opdracht uit:
    
    ```bash
    kubectl get service todoapp-service
    ```
    
    Werk de DNS-naam voor de bijbehorende IP-adres met de volgende opdracht:

    ```bash
    az network public-ip update --dns-name aks-todoapp --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Herhaal de aanroep voor `todoapp-test-blue` en `todoapp-test-green`:

    ```bash
    az network public-ip update --dns-name todoapp-blue --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>

    az network public-ip update --dns-name todoapp-green --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    De DNS-naam moet uniek zijn in uw abonnement. Om ervoor te zorgen dat, kunt u `<your-dns-name-suffix>`.

### <a name="create-an-instance-of-container-registry"></a>Maak een instantie van de Container Registry

1. Voer de `az acr create` opdracht om een exemplaar van Containerregister te maken. In de volgende sectie, kunt u vervolgens `login server` als de URL van de Docker-register.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Voer de `az acr credential` opdracht om de referenties van uw Container Registry weer te geven. Houd er rekening mee de Docker-register gebruikersnaam en wachtwoord, zoals u ze nodig in de volgende sectie hebt.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>De Jenkins-server voorbereiden

In deze sectie ziet u hoe bereid ik de Jenkins-server om uit te voeren van een build, dit prima voor testen is. Echter, moet u een [Azure VM-agent](https://plugins.jenkins.io/azure-vm-agents) of [Azure Container agent](https://plugins.jenkins.io/azure-container-agents) opzetten van een agent in Azure voor het uitvoeren van uw builds. Zie voor meer informatie het artikel Jenkins op de [invloed op de beveiliging van het bouwen van op master](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master).

1. Implementeer een [Jenkins-Master op Azure](https://aka.ms/jenkins-on-azure).

1. Verbinding maken met de server via SSH en de build-hulpprogramma's installeren op de server waarop u uw build.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Docker installeren](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Zorg ervoor dat de gebruiker `jenkins` is gemachtigd om uit te voeren de `docker` opdrachten.

1. [Kubectl installeren](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Download jq](https://stedolan.github.io/jq/download/).

1. Jq installeren met de volgende opdracht:

   ```bash
   sudo apt-get install jq
   ```
   
1. De invoegtoepassingen in Jenkins installeren door de volgende stappen uit in het dashboard van Jenkins:

    1. Selecteer **Jenkins beheren > invoegtoepassingen beheren > beschikbaar**.
    1. Zoek en installeer de invoegtoepassing Azure Container Service.

1. Referenties voor het beheren van resources in Azure toevoegen. Als u niet de invoegtoepassing al hebt, installeert u de **Azure Credential** invoegtoepassing.

1. Uw Azure-Service-Principal-referentie toevoegen als het type **Microsoft Azure Service-Principal**.

1. Voeg uw Azure Docker-register gebruikersnaam en het wachtwoord (zoals hebt verkregen in de sectie 'Een exemplaar van de Container Registry maken') als het type **gebruikersnaam met wachtwoord**.

## <a name="edit-the-jenkinsfile"></a>De Jenkinsfile bewerken

1. Ga in uw eigen opslagplaats naar `/deploy/aks/`, en open `Jenkinsfile`.

2. Het bestand als volgt bijwerken:

    ```groovy
    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = '<your-resource-group-name>'
    def aks = '<your-kubernetes-cluster-name>'

    def cosmosResourceGroup = '<your-cosmodb-resource-group>'
    def cosmosDbName = '<your-cosmodb-name>'
    def dbName = '<your-dbname>'

    def dockerRegistry = '<your-acr-name>.azurecr.io'
    ```
    
    Update voor de Container Registry referentie-ID:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>De taak maken
1. Toevoegen van een nieuwe taak in het type **pijplijn**.

1. Selecteer **pijplijn** > **definitie** > **Pipeline-script uit SCM**.

1. Voer de URL van de SCM-opslagplaats met uw &lt;uw gesplitst-opslagplaats >.

1. Voer in het scriptpad als `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>De taak uitvoeren

1. Controleer of dat u kunt uw project is uitgevoerd in uw lokale omgeving. Hier volgt hoe: [project uitvoeren op lokale computer](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it).

1. De Jenkins-taak uitvoeren. De eerste keer dat u de taak uitvoert implementeert Jenkins de taken-app naar de blauwe omgeving, die de standaardomgeving voor niet-actief is. 

1. Om te controleren of de taak wordt uitgevoerd, gaat u naar deze URL's:
    - Openbare eindpunt: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Blauwe eindpunt- `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Groen eindpunt- `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

De openbare en de blauwe test-eindpunten hebben dezelfde update, terwijl het groene eindpunt ziet u de standaardinstallatiekopie van tomcat. 

Als u meer dan één keer de build uitvoert, wordt het bladeren door blauw en groen implementaties. Met andere woorden, als de huidige omgeving blauw is, de taak wordt geïmplementeerd en de groene omgeving wordt gecontroleerd. Als de tests zijn goede, de taak werkt vervolgens het openbare eindpunt van de toepassing om verkeer te routeren aan de groene omgeving.

## <a name="additional-information"></a>Aanvullende informatie

Zie voor meer informatie over de implementatie zonder enige uitvaltijd, [quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de resources die u in deze zelfstudie hebt gemaakt niet meer nodig hebt, kunt u ze verwijderen.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen ondervindt met de Jenkins-invoegtoepassingen, kunt u in [Jenkins JIRA](https://issues.jenkins-ci.org/) een ticket openen voor het specifieke onderdeel.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u kunt implementeren naar AKS met behulp van Jenkins en het patroon blue/green implementatie. Zie voor meer informatie over de Azure Jenkins-provider, de via Jenkins op Azure-site.

> [!div class="nextstepaction"]
> [Jenkins op Azure](/azure/jenkins/)