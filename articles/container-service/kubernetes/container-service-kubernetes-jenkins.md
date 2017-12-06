---
title: Jenkins CI/CD met Kubernetes in Azure Containerservice
description: Hoe u een CI/CD automatiseren met Jenkins te implementeren en een beperkte app op Kubernetes in Azure Container Service bijwerken
services: container-service
author: chzbrgr71
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: cd8f7ae584b6b1afd357cc585df28dedd3c1f70e
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Jenkins integratie met Azure Container Service en Kubernetes 

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

In deze zelfstudie wordt het proces voor het instellen van continue integratie van een toepassing met meerdere container in Azure Container Service Kubernetes met behulp van het platform Jenkins doorlopen. De werkstroom de installatiekopie van de container in Docker-Hub updates en upgrades van het gehele Kubernetes product met behulp van de implementatie van een implementatie. 

## <a name="high-level-process"></a>Proces van hoog niveau
De eenvoudige stappen in dit artikel wordt beschreven, zijn: 
- Een cluster Kubernetes in Container Service installeren
- Jenkins instellen en configureren van toegang tot de Container Service
- Een werkstroom Jenkins maken
- Het proces CI/CD complete testen

## <a name="install-a-kubernetes-cluster"></a>Een cluster Kubernetes installeren
    
Implementeer het Kubernetes-cluster in Azure Container Service met de volgende stappen. Volledige documentatie bevindt [hier](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>Stap 1: Een resourcegroep maken
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a>Stap 2: Implementeer het cluster
> [!NOTE]
> De volgende stappen moet een lokale openbare SSH-sleutel met de opgeslagen in de map ~/.ssh.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a>Jenkins instellen en configureren van toegang tot de Container Service

### <a name="step-1-install-jenkins"></a>Stap 1: Jenkins installeren
1. Een virtuele machine in Azure maken met Ubuntu 16.04 TNS.  Aangezien u verbinding maken met deze virtuele machine met behulp van bash op uw lokale machine later in de stappen wilt, het 'verificatietype' naar 'Openbare SSH-sleutel' ingesteld en de openbare SSH-sleutel die lokaal wordt opgeslagen in de map ~/.ssh plakken.  Ook rekening met de gebruikersnaam die u opgeeft, aangezien deze gebruikersnaam nodig om het dashboard Jenkins weer te geven en voor het verbinden met de Jenkins VM in latere stappen.
2. Jenkins installeren via deze [instructies](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Een zelfstudie voor meer gedetailleerde loopt [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. Bijwerken om weer te geven het dashboard Jenkins op uw lokale computer, de beveiligingsgroep voor de Azure-netwerk dat poort 8080 door een inkomende regel waarmee toegang tot poort 8080 toe te voegen.  U mogelijk ook poort doorsturen instellen door deze opdracht uit te voeren:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Verbinding maken met uw Jenkins-server met behulp van de browser door te navigeren naar het openbare IP-adres (http:// < your_jenkins_public_ip >: 8080) en het ontgrendelen van het dashboard Jenkins voor de eerste keer met de initiële beheerderswachtwoord.  De Administrator-wachtwoord wordt opgeslagen op /var/lib/jenkins/secrets/initialAdminPassword op de Jenkins VM.  Voeg SSH toe aan de Jenkins VM is een eenvoudige manier om dit wachtwoord te achterhalen: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Voer vervolgens: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Docker installeren op de machine Jenkins via deze [instructies](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). Hierdoor Docker-opdrachten in Jenkins taken worden uitgevoerd.
6. Docker-machtigingen om toe te staan Jenkins voor toegang tot het Docker-eindpunt configureren.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Installeer `kubectl` CLI op Jenkins. Meer informatie vindt u op [installeren en instellen van kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Jenkins taken te beheren en implementeren met het cluster Kubernetes 'kubectl' gebruikt.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a>Stap 2: Toegang tot het cluster Kubernetes instellen

> [!NOTE]
> Er zijn meerdere methoden voor het uitvoeren van de volgende stappen uit. Gebruik de methode die voor u.
>

1. Kopieer de `kubectl` config van het bestand in de machine Jenkins zodat Jenkins taken hebben toegang tot het cluster Kubernetes. Deze instructies wordt ervan uitgegaan dat u gebruikmaakt van een andere computer dan de Jenkins VM bash en een lokale openbare SSH-sleutel in de machine ~/.ssh map is opgeslagen.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Valideren van Jenkins dat het cluster Kubernetes toegankelijk is.  Om dit te doen, SSH in de VM Jenkins: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Controleer vervolgens of Jenkins verbinding kan maken met uw cluster: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Een werkstroom Jenkins maken

### <a name="prerequisites"></a>Vereisten

- GitHub-account voor code-opslagplaats.
- Docker Hub account op te slaan en afbeeldingen bijwerken.
- Beperkte toepassing die kan worden opnieuw opgebouwd en bijgewerkt. U kunt deze container voorbeeldapp geschreven in Golang: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> De volgende stappen moeten worden uitgevoerd in uw eigen GitHub-account. Kloon de bovenstaande opslagplaats gerust maar moet u uw eigen account gebruiken voor het configureren van de webhooks en Jenkins toegang.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>Stap 1: Initiële v1 van toepassing implementeren
1. Het bouwen van de app uit de developer-machine met de volgende opdrachten. Vervang `myrepo` door uw eigen.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Push-afbeelding met Docker-Hub.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Op het cluster Kubernetes implementeren.
    
    > [!NOTE] 
    > Bewerk de `go-web.yaml` bestand bijwerken van uw installatiekopie van de container en de opslagplaats.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>Stap 2: Jenkins systeem configureren
1. Klik op **beheren Jenkins** > **systeem configureren**.
2. Onder **GitHub**, selecteer **GitHub-Server toevoegen**.
3. Laat **URL van de API** als standaard.
4. Onder **referenties**, Voeg een Jenkins referentie met **geheime tekst**. U wordt aangeraden met GitHub persoonlijke toegangstokens, die zijn geconfigureerd in de instellingen van uw GitHub-gebruikersaccount. Meer informatie over dit [hier.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Klik op **verbinding testen** om te controleren of dit juist is geconfigureerd.
6. Onder **algemene eigenschappen**, voegt u een omgevingsvariabele `DOCKER_HUB` en geef uw wachtwoord Docker-Hub. (Dit is handig in deze demonstratie, maar een productiescenario vereist een veiliger benadering.)
7. Opslaan.

![Jenkins GitHub-toegang](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a>Stap 3: De werkstroom Jenkins maken
1. Een item Jenkins maken.
2. Geef een naam (bijvoorbeeld 'Ga web') en selecteer **Freestyle Project**. 
3. Controleer **GitHub project** en geef de URL naar uw GitHub-opslagplaats.
4. In **beheer van gegevensbronnen Code**, de GitHub-repo-URL en referenties opgeven. 
5. Voeg een **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Toevoegen van een andere **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins bouwen stappen](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Het item Jenkins opslaan en testen met **nu bouwen**.

### <a name="step-4-connect-github-webhook"></a>Stap 4: Verbinding maken met GitHub webhook
1. Klik in het Jenkins item dat u hebt gemaakt, op **configureren**.
2. Onder **bouwen Triggers**, selecteer **GitHub haakje trigger voor polling GITScm** en **opslaan**. Hiermee worden de GitHub webhook automatisch geconfigureerd.
3. Klik in uw GitHub-repo voor Ga-webtoepassingen op **instellingen > Webhooks**.
4. Controleer of de Jenkins webhook-URL is toegevoegd. De URL moet eindigen op 'github webhook'.

![Configuratie van de webhook Jenkins](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a>Het proces CI/CD complete testen

1. Code voor de opslagplaats en push/synch bijwerken met de GitHub-opslagplaats.
2. Controleer op de console Jenkins de **bouwen geschiedenis** en controleert u of de taak is uitgevoerd. Console-uitvoer weergeven om details te bekijken.
3. Details van de bijgewerkte implementatie Kubernetes, bekijken:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Volgende stappen

- Azure Container register implementeren en afbeeldingen in een veilige locatie opslaan. Zie [Azure Container register docs](https://docs.microsoft.com/azure/container-registry).
- Bouw een complexe workflow met side-by-side-implementatie en automatische tests in Jenkins.
- Zie voor meer informatie over CI/CD met Jenkins en Kubernetes de [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
