---
title: Zelfstudie - Azure Kubernetes Service (AKS) met Jenkins implementeren vanuit GitHub
description: Jenkins instellen voor continue integratie (CI) van GitHub en continue implementatie (CD) naar Azure Kubernetes Service (AKS)
services: container-service
ms.service: container-service
author: iainfoulds
ms.author: iainfou
ms.topic: article
ms.date: 09/27/2018
ms.openlocfilehash: d252e275280ed2a5c2129f6b228e9989a33b37fd
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853612"
---
# <a name="tutorial-deploy-from-github-to-azure-kubernetes-service-aks-with-jenkins-continuous-integration-and-deployment"></a>Zelfstudie: Implementeren vanuit GitHub op Azure Kubernetes Service (AKS) met Jenkins continue integratie en implementatie

In deze zelfstudie implementeert u een voorbeeld-app vanuit GitHub naar een [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes) cluster door het instellen van continue integratie (CI) en continue implementatie (CD) in Jenkins. Op die manier tijdens het bijwerken van uw app door doorvoeracties te pushen naar GitHub, Jenkins automatisch een nieuwe build van container wordt uitgevoerd, pushes containerinstallatiekopieën naar Azure Container Registry (ACR) en voert u uw app in AKS. 

In deze zelfstudie, moet u deze taken uitvoeren:

> [!div class="checklist"]
> * Een voorbeeld Azure vote-app implementeren in een AKS-cluster.
> * Maak een eenvoudige Jenkins-project.
> * Referenties voor Jenkins om te communiceren met ACR instellen.
> * Maak een Jenkins build-taak en de GitHub-webhook voor geautomatiseerde builds.
> * Test de CI/CD-pijplijn voor het bijwerken van een toepassing in AKS op basis van de GitHub-doorvoeracties code.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie, moet u deze items:

- Basiskennis van Kubernetes, Git, CI/CD en container-installatiekopieën

- Een [AKS-cluster] [ aks-quickstart] en `kubectl` geconfigureerd met de [AKS-cluster referenties][aks-credentials]

- Een [Azure Container Registry (ACR) register][acr-quickstart], de naam van de ACR-aanmeldingsserver en het AKS-cluster dat is geconfigureerd voor het [verifiëren bij het ACR-register][acr-authentication]

- Azure CLI versie 2.0.46 of hoger is geïnstalleerd en geconfigureerd. Voer `az --version` de versie te vinden. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

- [Docker is geïnstalleerd] [ docker-install] op uw ontwikkelingssysteem

- Een GitHub-account, [persoonlijk GitHub-toegangstoken][git-access-token], en Git-client is geïnstalleerd op uw ontwikkelingssysteem

- Als u uw eigen Jenkins-exemplaar in plaats van dit voorbeeld in een script vastgelegd manier voor het implementeren van Jenkins, uw Jenkins-exemplaar behoeften [Docker geïnstalleerd en geconfigureerd] [ docker-install] en [kubectl][kubectl-install].

## <a name="prepare-your-app"></a>Uw app voorbereiden

In dit artikel gebruikt u een voorbeeld van Azure vote-toepassing die een webinterface die wordt gehost op een of meer schillen en een tweede schil hosting van Redis voor opslag van tijdelijke gegevens bevat. Voordat u Jenkins en AKS voor geautomatiseerde implementaties integreert, eerst handmatig voorbereiden en implementeren van de Azure vote-toepassing naar uw AKS-cluster. Deze handmatige implementatie versie 1 van de toepassing is en kunt u de toepassing in actie zien.

De volgende GitHub-opslagplaats voor de voorbeeldtoepassing - splitsen [ https://github.com/Azure-Samples/azure-voting-app-redis ](https://github.com/Azure-Samples/azure-voting-app-redis). Om de opslagplaats naar uw eigen GitHub-account te vertakken, selecteert u de knop **Fork** in de rechterbovenhoek.

Kloon de fork ook naar uw ontwikkelsysteem. Zorg ervoor dat u de URL van uw fork gebruiken bij het klonen van deze opslagplaats:

```console
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Ga naar de map van de gekloonde fork:

```console
cd azure-voting-app-redis
```

Voor het maken van de containerinstallatiekopieën die nodig zijn voor de voorbeeldtoepassing gebruikt de *docker-compose.yaml* bestand met `docker-compose`:

```console
docker-compose up -d
```

De vereiste basisinstallatiekopieën worden opgehaald en de toepassingscontainers gebouwd. Vervolgens kunt u de [docker-installatiekopieën] [ docker-images] opdracht om de gemaakte installatiekopie te bekijken. Er zijn drie installatiekopieën gedownload of gemaakt. De `azure-vote-front`-installatiekopie bevat de toepassing en gebruikt de `nginx-flask`-installatiekopie als basis. De `redis` installatiekopie wordt gebruikt om een Redis-exemplaar te starten:

```
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Voordat u kunt pushen de *azure-vote-front* container-installatiekopie naar ACR, krijgen uw ACR-aanmeldingsserver met de [az acr list] [ az-acr-list] opdracht. Het volgende voorbeeld wordt het adres van de ACR-aanmelding server voor een register in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Gebruik de [docker tag] [ docker-tag] opdracht voor het taggen van de installatiekopie met de naam van de ACR-aanmeldingsserver en een uniek versienummer van `v1`. Geef uw eigen `<acrLoginServer>` naam in de vorige stap verkregen:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Ten slotte, push de *azure-vote-front* installatiekopie naar het ACR-register. Vervang weer `<acrLoginServer>` met de naam van de aanmeldingsserver van uw eigen ACR-register, zoals `myacrregistry.azurecr.io`:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-the-sample-application-to-aks"></a>De voorbeeldtoepassing met AKS implementeren

Voor het implementeren van de voorbeeld-App naar uw AKS-cluster, kunt u de Kubernetes-manifestbestand in de hoofdmap van de Azure vote-opslagplaats-opslagplaats. Open de *azure-vote-all-in-one-redis.yaml* manifestbestand met een editor zoals `vi`. Vervang `microsoft` door de naam van de ACR-aanmeldingsserver. Deze waarde is gevonden op regel **47** van het manifestbestand:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Gebruik vervolgens de [kubectl toepassen] [ kubectl-apply] opdracht om de toepassing naar uw AKS-cluster te implementeren:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Een Kubernetes-service voor load balancer is gemaakt als de toepassing op internet beschikbaar wilt maken. Dit proces kan enkele minuten duren. U kunt de voortgang van de load balancer-implementatie controleren met de [kubectl service ophalen] [ kubectl-get] opdracht met de `--watch` argument. Zodra het *EXTERNE IP-adres* is gewijzigd van *in behandeling* in een *IP-adres*, gebruikt u `Control + C` om het controleproces van kubectl te stoppen.

```console
$ kubectl get service azure-vote-front --watch

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.215.27   <pending>     80:30747/TCP   22s
azure-vote-front   LoadBalancer   10.0.215.27   40.117.57.239   80:30747/TCP   2m
```

Als u wilt zien van de toepassing in actie, open een webbrowser naar het externe IP-adres van uw service. De Azure vote-toepassing wordt weergegeven, zoals wordt weergegeven in het volgende voorbeeld:

![Voorbeeld van Azure vote-toepassing die wordt uitgevoerd in AKS](media/aks-jenkins/azure-vote.png)

## <a name="deploy-jenkins-to-an-azure-vm"></a>Jenkins implementeren op een Azure-VM

Als u wilt snel Jenkins implementeren voor gebruik in dit artikel, kunt u het volgende script naar een Azure-machine implementeren, configureren van toegang tot het netwerk en een eenvoudige Jenkins-installatie te voltooien. Voor verificatie tussen Jenkins en het AKS-cluster kopieert het script het Kubernetes-configuratiebestand vanuit het ontwikkelsysteem naar het Jenkins-systeem.

> [!WARNING]
> Met dit voorbeeldscript wordt voor deze demo voor het snel inrichten van een Jenkins-omgeving die wordt uitgevoerd op een Azure-VM. Het maakt gebruik van de aangepaste scriptextensie van Azure voor het configureren van een virtuele machine en vervolgens de vereiste referenties weer te geven. Uw *~/.kube/config* wordt gekopieerd naar de Jenkins-VM.

Voer de volgende opdrachten downloaden en uitvoeren van het script. U wordt aangeraden de inhoud van een script voor vóór het uitvoeren van deze- [ https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh ](https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh).

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Het duurt een paar minuten aan de virtuele machine maken en implementeren van de vereiste onderdelen voor Docker en Jenkins. Wanneer het script is voltooid, voert een adres voor de Jenkins-server en een sleutel voor het ontgrendelen van het dashboard, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
Open a browser to http://40.115.43.83:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

Open een webbrowser naar de URL die wordt weergegeven en voer de unlock-sleutel. Volg de aanwijzingen om de Jenkins-configuratie te voltooien:

- Kies **Installeer voorgestelde invoegtoepassingen**
- Maak de eerste gebruiker met beheerdersrechten. Voer een gebruikersnaam, zoals *azureuser*, geeft u een veilig wachtwoord. Als laatste typt u een volledige naam en e-mailadres.
- Selecteer **Save and Finish**
- Zodra Jenkins gereed is, selecteert u **Start using Jenkins**
    - Als uw webbrowser een lege pagina weergeeft wanneer u Jenkins gaat gebruiken, moet u de Jenkins-service opnieuw starten. De service te starten, SSH naar het openbare IP-adres van uw Jenkins-exemplaar en het type `sudo service jenkins restart`. Nadat de service opnieuw is opgestart, vernieuwt u webbrowser.
- Meld u met Jenkins met de gebruikersnaam en het wachtwoord dat u hebt gemaakt in het installatieproces.

## <a name="create-a-jenkins-environment-variable"></a>Een Jenkins-omgevingsvariabele maken

Een Jenkins-omgevingsvariabele wordt gebruikt voor het opslaan van de naam van de ACR-aanmeldingsserver. Deze variabele wordt verwezen tijdens de Jenkins-build-taak. Voor het maken van deze omgevingsvariabele, voert u de volgende stappen uit:

- Aan de linkerkant van de Jenkins-portal, selecteert u **Manage Jenkins** > **System configureren**
- Onder **algemene eigenschappen**, selecteer **omgevingsvariabelen**. Toevoegen van een variabele met de naam van de `ACR_LOGINSERVER` en de waarde van uw ACR-aanmeldingsserver.

    ![Jenkins-omgevingsvariabelen](media/aks-jenkins/env-variables.png)

- Als u klaar bent, klikt u op **opslaan** onderaan de pagina van de configuratie van Jenkins.

## <a name="create-a-jenkins-credential-for-acr"></a>Een Jenkins-referentie voor ACR maken

Als u wilt toestaan dat Jenkins voor het bouwen en vervolgens bijgewerkte containerinstallatiekopieën naar ACR te pushen, moet u referenties opgeven voor ACR. Deze verificatie kan gebruikmaken van Azure Active Directory service-principals. In de vereisten, u de service-principal geconfigureerd voor uw AKS-cluster met *lezer* machtigingen voor uw ACR-register. Deze rechten geven de AKS-cluster naar *pull* installatiekopieën van het ACR-register. Tijdens het proces CI/CD Jenkins bouwt nieuwe containerinstallatiekopieën op basis van toepassingsupdates, en vervolgens moet *push* deze installatiekopieën naar het ACR-register. Voor scheiding van rollen en machtigingen, configureert u nu een service-principal voor Jenkins met *Inzender* machtigingen voor uw ACR-register.

### <a name="create-a-service-principal-for-jenkins-to-use-acr"></a>Maken van een service-principal voor Jenkins ACR gebruiken

Maak eerst een service principal met de [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] opdracht:

```azurecli
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "626dd8ea-042d-4043-a8df-4ef56273670f",
  "displayName": "azure-cli-2018-09-28-22-19-34",
  "name": "http://azure-cli-2018-09-28-22-19-34",
  "password": "1ceb4df3-c567-4fb6-955e-f95ac9460297",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Noteer de *appId* en *wachtwoord* wordt weergegeven in de uitvoer. Deze waarden worden gebruikt in de volgende stappen uit om te configureren van de referentie-resource in Jenkins.

Haal de resource-ID van uw ACR-register met de [az acr show] [ az-acr-show] opdracht, en op te slaan als een variabele. Geef uw naam van de resourcegroep en de ACR-naam:

```azurecli
ACR_ID=$(az acr show --resource-group myResourceGroup --name <acrLoginServer> --query "id" --output tsv)
```

Maak nu een roltoewijzing voor de service-principal toewijzen *Inzender* rechten voor het ACR-register. In het volgende voorbeeld, Geef uw eigen *appId* weergegeven in de uitvoer van een vorige opdracht om de serviceprincipal te maken:

```azurecli
az role assignment create --assignee 626dd8ea-042d-4043-a8df-4ef56273670f --role Contributor --scope $ACR_ID
```

### <a name="create-a-credential-resource-in-jenkins-for-the-acr-service-principal"></a>Een referentie-resource in Jenkins voor het ACR-service-principal maken

Met de roltoewijzing is gemaakt in Azure, moet u nu uw ACR-referenties opslaan in een Jenkins-referentieobject. Deze referenties worden verwezen bij de Jenkins-build-taak.

Terug aan de linkerkant van de Jenkins-portal, klikt u op **referenties** > **Jenkins** > **algemene aanmeldgegevens (onbeperkte)**  >  **Referenties toevoegen**

Zorg ervoor dat het type referentie **gebruikersnaam met wachtwoord** en voer de volgende items:

- **Gebruikersnaam** : de *appId* van de service-principal gemaakt voor verificatie met uw ACR-register.
- **Wachtwoord** : de *wachtwoord* van de service-principal gemaakt voor verificatie met uw ACR-register.
- **ID** -referentie-id zoals *acr-referenties*

Als u klaar bent, wordt het formulier referenties ziet eruit zoals in het volgende voorbeeld:

![Een Jenkins-referentieobject maken met de gegevens voor de service-principal](media/aks-jenkins/acr-credentials.png)

Klik op **OK** en Ga terug naar de Jenkins-portal.

## <a name="create-a-jenkins-project"></a>Een Jenkins-project maken

Selecteer in de startpagina van uw Jenkins-portal, **nieuw item** aan de linkerkant:

1. Voer *azure-vote* als de taaknaam van de. Kies **Freestyle project**en selecteer vervolgens **OK**
1. Selecteer onder de sectie **Algemeen** de optie **GitHub-project** en voer de URL van uw gevorkte opslagplaats in, zoals *https://github.com/\<your-github-account\>/azure-voting-app-redis*
1. Selecteer onder de sectie **Broncodebeheer** de optie **Git** en voer de *.git*-URL van uw gevorkte opslagplaats in, zoals *https://github.com/\<your-github-account\>/azure-voting-app-redis.git*
    - Klik op voor de referenties en **toevoegen** > **Jenkins**
    - Onder **soort**, selecteer **geheime tekst** en voer uw [persoonlijk GitHub-toegangstoken] [ git-access-token] als de geheime sleutel.
    - Selecteer **toevoegen** wanneer u klaar bent.

    ![GitHub-referenties](media/aks-jenkins/github-creds.png)

1. Onder de **Triggers bouwen** sectie, selecteer **GitHub-hookactivatie voor GITscm-polling**
1. Onder **omgeving bouwen**, selecteer **geheime teksten of bestanden gebruiken**
1. Onder **bindingen**, selecteer **toevoegen** > **gebruikersnaam en wachtwoord (gescheiden)**
    - Voer `ACR_ID` voor de **Gebruikersnaamvariabele**, en `ACR_PASSWORD` voor de **Wachtwoordvariabele**

    ![Jenkins-bindingen](media/aks-jenkins/bindings.png)

1. Kies de optie om toe te voegen een **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst. Met dit script maakt een nieuwe containerinstallatiekopie en wordt naar uw ACR-register gepusht.

    ```bash
    # Build new image and push to ACR.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME ./azure-vote
    docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
    docker push $WEB_IMAGE_NAME
    ```

1. Voeg nog een **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst. Met dit script wordt de implementatie van de toepassing in AKS bijgewerkt met de nieuwe containerinstallatiekopie vanuit ACR.

    ```bash
    # Update kubernetes deployment with new image.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

1. Wanneer voltooid, klikt u op **opslaan**.

## <a name="test-the-jenkins-build"></a>Testen van de Jenkins-build

Voordat u de taak op basis van de GitHub-doorvoeracties automatiseren, eerst handmatig test de Jenkins-build. Deze handmatige build valideert dat de taak correct is geconfigureerd, de juiste verificatiebestand van Kubernetes is voldaan en dat de verificatie met ACR werkt.

Selecteer op het menu links in het project **Build Now**.

![Jenkins testen build](media/aks-jenkins/test-build.png)

De eerste build duurt een minuut of twee als de Docker-installatiekopie-lagen worden opgevraagd met de Jenkins-server. Volgende builds kunnen de lagen in de cache installatiekopie gebruiken voor het verbeteren van de build-tijden.

Tijdens het bouwproces wordt de GitHub-opslagplaats gekloond naar de Jenkins-build-server. Een nieuwe containerinstallatiekopie is gemaakt en naar het ACR-register gepusht. Ten slotte wordt de Azure vote-toepassing die wordt uitgevoerd op het AKS-cluster bijgewerkt voor het gebruik van de nieuwe installatiekopie. Omdat er geen wijzigingen hebt aangebracht in de toepassingscode, wordt de toepassing niet gewijzigd als u de voorbeeld-app in een webbrowser weergeven.

Als de build-taak voltooid is, klikt u op **bouwen #1** onder build-geschiedenis. Selecteer **Console-uitvoer** en geef de uitvoer van het bouwproces. De laatste regel moet een build aangeven.

## <a name="create-a-github-webhook"></a>Een GitHub-webhook maken

Met een handmatige build voltooid, moet u nu GitHub integreren in de Jenkins-build. Een webhook kan worden gebruikt om uit te voeren van de Jenkins-build-taak telkens wanneer die een code-doorvoer wordt gemaakt in GitHub. Voor het maken van de GitHub-webhook, voert u de volgende stappen uit:

1. Blader naar uw Gevorkte GitHub-opslagplaats in een webbrowser.
1. Selecteer **instellingen**en selecteer vervolgens **Webhooks** aan de linkerkant.
1. Kies aan **webhook toevoegen**. Voor de *nettolading URL*, voer `http://<publicIp:8080>/github-webhook/`, waarbij `<publicIp>` is het IP-adres van de Jenkins-server. Zorg ervoor dat u de afsluitende /. Laat de overige standaardwaarden voor type inhoud en activeren op *push* gebeurtenissen.
1. Selecteer **webhook toevoegen**.

    ![Een GitHub-webhook maken voor Jenkins](media/aks-jenkins/webhook.png)

## <a name="test-the-complete-cicd-pipeline"></a>De volledige CI/CD-pijplijn testen

U kunt nu de volledige CI/CD-pijplijn testen. Wanneer u een code-doorvoer naar GitHub pushen, gebeuren de volgende stappen uit:

1. De GitHub-webhook gezocht naar Jenkins.
1. Jenkins build-taak wordt gestart en haalt de meest recente code doorvoeren van GitHub.
1. Een Docker-build is gestart met de bijgewerkte code en de nieuwe containerinstallatiekopie is gemarkeerd met de meest recente build-nummer.
1. Deze nieuwe containerinstallatiekopie is gepusht naar Azure Container Registry.
1. Uw toepassing geïmplementeerd in Azure Kubernetes Service-updates met de meest recente containerinstallatiekopie uit het register van de Azure Container Registry.

Op uw ontwikkelcomputer, opent u de gekloonde toepassing met een code-editor. Onder de */azure-vote/azure-vote* directory, open het bestand met de naam **config_file.cfg**. Werk de waarden stem in dit bestand op een andere waarde dan katten en honden, zoals wordt weergegeven in het volgende voorbeeld:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Als u bijwerkt, sla het bestand, de wijzigingen aan en push deze naar uw fork van de GitHub-opslagplaats. De GitHub-webhook activeert een nieuwe build-taak in Jenkins. In het dashboard van Jenkins web bewaakt het bouwproces. Het duurt een paar seconden naar de meest recente code op te halen, maken en de bijgewerkte installatiekopie pushen en implementeer de bijgewerkte toepassing in AKS.

Nadat de build voltooid is, vernieuwt u de webbrowser van het voorbeeld van Azure vote-toepassing. Uw wijzigingen worden weergegeven, zoals wordt weergegeven in het volgende voorbeeld:

![Azure-voorbeeld stemmen in AKS bijgewerkt door de Jenkins-build-taak](media/aks-jenkins/azure-vote-updated.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u Jenkins gebruikt als onderdeel van een CI/CD-oplossing. AKS kunt integreren met andere CI/CD-oplossingen en hulpprogramma's voor werkstroomautomatisering, zoals de [Azure DevOps-Project] [ azure-devops] of [het maken van een AKS-cluster met Ansible] [ aks-ansible].

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[docker-install]: https://docs.docker.com/install/
[kubectl-install]: https://kubernetes.io/docs/tasks/tools/install-kubectl/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az-acr-list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md#grant-aks-access-to-acr
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli
[install-azure-cli]: /cli/azure/install-azure-cli
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-acr-show]: /cli/azure/acr#az-acr-show
[azure-devops]: ../devops-project/azure-devops-project-aks.md
[aks-ansible]: ../ansible/ansible-create-configure-aks.md
