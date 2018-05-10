---
title: Continue implementatie met Kubernetes in Azure Kubernetes Service Jenkins
description: Het automatiseren van een proces continue implementatie met Jenkins te implementeren en een beperkte app op Kubernetes in Azure Kubernetes Service bijwerken
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: efdb89c5f4c6bdb9b007b7c0020cbdb8f6034eed
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="continuous-deployment-with-jenkins-and-azure-kubernetes-service"></a>Continue implementatie met Jenkins en Azure Kubernetes Service

Dit document wordt gedemonstreerd hoe een eenvoudige continue implementatiewerkstroom tussen Jenkins en een Azure Kubernetes Service (AKS)-cluster instellen.

De voorbeeldwerkstroom bevat de volgende stappen uit:

> [!div class="checklist"]
> * De toepassing Azure stem voor uw cluster Kubernetes implementeren.
> * Werk de toepassingscode Azure stem en push naar een GitHub-opslagplaats, wordt opgestart continue implementatie.
> * Jenkins kloont de opslagplaats en de installatiekopie van een nieuwe container met de bijgewerkte code is gebaseerd.
> * Deze installatiekopie geduwd naar Azure Container register (ACR).
> * De toepassing die wordt uitgevoerd in het cluster AKS wordt bijgewerkt met de nieuwe installatiekopie van de container.

## <a name="prerequisites"></a>Vereisten

U kunt de volgende items nodig om de stappen in dit artikel te voltooien.

- Basiskennis van Kubernetes, Git CI/CD en Azure Container register (ACR).
- Een [Azure Kubernetes Service (AKS)-cluster] [ aks-quickstart] en [AKS referenties geconfigureerd] [ aks-credentials] op uw ontwikkelsysteem.
- Een [Azure Container register (ACR) register][acr-quickstart], de servernaam ACR aanmelding en [ACR referenties] [ acr-authentication] met push als pull-toegang.
- Azure CLI op uw ontwikkelsysteem geïnstalleerd.
- Docker geïnstalleerd op uw ontwikkelsysteem.
- GitHub-account [GitHub persoonlijke toegangstoken][git-access-token], en Git-client is geïnstalleerd op uw ontwikkelsysteem.

## <a name="prepare-application"></a>Voorbereiden van de toepassing

De stem van Azure-toepassing in dit hele document bevat een webinterface die wordt gehost in een of meer gehele product en een tweede schil Redis als host fungeert voor de tijdelijke opslag.

Voordat de Jenkins / AKS integratie, voorbereiden en implementeren van de toepassing Azure stem met uw cluster AKS. Dit beschouwen als versie 1 van de toepassing.

De volgende GitHub-opslagplaats vertakken.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Zodra de fork is gemaakt, maar kloon uw ontwikkelsysteem. Zorg ervoor dat u de URL van uw fork gebruikt bij het klonen van deze opslagplaats.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Wijzig de mappen zodat u vanuit de gekloonde map werkt.

```bash
cd azure-voting-app-redis
```

Voer de `docker-compose.yaml` bestand maken de `azure-vote-front` container installatiekopie en start de toepassing.

```bash
docker-compose up -d
```

Wanneer het voltooid, gebruiken de [docker-installatiekopieën] [ docker-images] opdracht om te zien van de installatiekopie gemaakt.

U ziet dat er drie installatiekopieën zijn gedownload of gemaakt. De `azure-vote-front`-installatiekopie bevat de toepassing en gebruikt de `nginx-flask`-installatiekopie als basis. De `redis`-installatiekopie wordt gebruikt om een Redis-exemplaar te starten.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Ophalen van de server ACR aanmelding met de [az acr lijst] [ az-acr-list] opdracht. Zorg ervoor dat bij het bijwerken van de naam van de resourcegroep met de resourcegroep die als host fungeert voor uw ACR-register.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Gebruik de [docker-tag] [ docker-tag] opdracht voor het taggen van de installatiekopie met de naam van de aanmeldings-server en een versienummer van `v1`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

De waarde ACR aanmelding server bijwerken met uw ACR server aanmeldingsnaam en push de `azure-vote-front` afbeelding in het register.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Implementatie van toepassing op Kubernetes

Een Kubernetes manifestbestand kan worden gevonden in de hoofdmap van de opslagplaats Azure stem en kan worden gebruikt om de toepassing naar uw cluster Kubernetes implementeren.

Werk eerst de **azure-vote-all-in-one-redis.yaml** manifestbestand met de locatie van uw ACR-register. Open het bestand met een teksteditor en vervang `microsoft` met de naam van ACR aanmelding. U vindt deze waarde op regel **47** van het manifestbestand.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Gebruik vervolgens de [kubectl maken] [ kubectl-create] opdracht de toepassing uit te voeren. Deze opdracht parseert het manifestbestand en maakt de gedefinieerde Kubernetes-objecten.

```bash
kubectl create -f azure-vote-all-in-one-redis.yaml
```

Een [Kubernetes service] [ kubernetes-service] wordt gemaakt om de toepassing met het internet zichtbaar te maken. Dit proces kan enkele minuten duren.

Gebruik de opdracht [kubectl get service][kubectl-get] met het argument `--watch` om de voortgang te controleren.

```bash
kubectl get service azure-vote-front --watch
```

Eerst wordt het *externe IP-adres* voor de service *azure-vote-front* weergegeven als *in behandeling*.

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Zodra het *EXTERNE IP-adres* is gewijzigd van *in behandeling* in een *IP-adres*, gebruikt u `control+c` om het controleproces van kubectl te stoppen.

```
azure-vote-front   10.0.34.242   13.90.150.118   80:30676/TCP   2m
```

Als u de toepassing wilt zien, bladert u naar het externe IP-adres.

![Afbeelding van Kubernetes-cluster in Azure](media/aks-jenkins/azure-vote-safari.png)

## <a name="deploy-jenkins-to-vm"></a>Jenkins implementeren voor VM

Een script is vooraf gemaakte naar een virtuele machine implementeren, toegang tot het netwerk configureren en een eenvoudige Jenkins-installatie te voltooien. Het script kopieert bovendien uw configuratiebestand Kubernetes van uw ontwikkelsysteem aan het systeem Jenkins. Dit bestand wordt gebruikt voor verificatie tussen Jenkins en het cluster AKS.

Voer de volgende opdrachten om te downloaden en voer het script. De onderstaande URL kan ook worden gebruikt om te controleren van de inhoud van het script.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Wanneer het script is voltooid, levert dit een adres voor de server Jenkins als goed sleutel voor het ontgrendelen van Jenkins. Blader naar de URL, voer de sleutel Volg hiervoor de aanwijzingen om de Jenkins-configuratie te voltooien.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Omgevingsvariabelen Jenkins

Een omgevingsvariabele Jenkins wordt gebruikt voor het Azure Container register (ACR) server aanmeldingsnaam bevatten. Deze variabele wordt verwezen tijdens het Jenkins continue implementatie.

Klik terwijl op de beheerportal Jenkins **beheren Jenkins** > **System configureren**.

Onder **algemene eigenschappen**, selecteer **omgevingsvariabelen**, en het toevoegen van een variabele met de naam `ACR_LOGINSERVER` en een waarde van de server van uw ACR aanmelding.

![Omgevingsvariabelen Jenkins](media/aks-jenkins/env-variables.png)

Wanneer voltooid, klikt u op **opslaan** op de pagina Jenkins configuratie.

## <a name="jenkins-credentials"></a>Jenkins referenties

Nu uw ACR-referenties opgeslagen in een Jenkins-referentieobject. Deze referenties wordt verwezen tijdens de Jenkins build-taak.

Klik in het beheerportal Jenkins op **referenties** > **Jenkins** > **globale referentie (onbeperkte)**  >   **Referenties toevoegen**.

Zorg ervoor dat het type referentie **gebruikersnaam met wachtwoord** en voer de volgende items:

- **Gebruikersnaam** -ID van de service principal gebruik voor de verificatie met uw ACR-register.
- **Wachtwoord** -clientgeheim van de service principal gebruiken voor verificatie met uw ACR-register.
- **ID** -referentie-id zoals `acr-credentials`.

Na voltooiing wordt het formulier referenties moet er ongeveer uitzien naar deze afbeelding:

![ACR-referenties](media/aks-jenkins/acr-credentials.png)

Klik op **OK** en terugkeren naar de Jenkins-beheerportal.

## <a name="create-jenkins-project"></a>Jenkins-project maken

Klik in het beheerportal Jenkins op **Nieuw Item**.

Geef het project een naam, bijvoorbeeld `azure-vote`, selecteer **Freestyle Project**, en klik op **OK**.

![Jenkins project](media/aks-jenkins/jenkins-project.png)

Onder **algemene**, selecteer **GitHub project** en voert u de URL naar uw fork van het Azure stem GitHub-project.

![GitHub-project](media/aks-jenkins/github-project.png)

Onder **Source Code Management**, selecteer **Git**, voert u de URL naar uw fork van de Azure-stem GitHub-repo.

Klik op voor de referenties en **toevoegen** > **Jenkins**. Onder **soort**, selecteer **geheime tekst** en voer uw [GitHub persoonlijke toegangstoken] [ git-access-token] als het geheim.

Selecteer **toevoegen** wanneer u klaar bent.

![GitHub-referenties](media/aks-jenkins/github-creds.png)

Onder **bouwen Triggers**, selecteer **GitHub haakje trigger voor GITScm polling**.

![Jenkins bouwen triggers](media/aks-jenkins/build-triggers.png)

Onder **bouwen omgeving**, selecteer **gebruik geheime tekst of bestanden**.

![Jenkins build-omgeving](media/aks-jenkins/build-environment.png)

Onder **bindingen**, selecteer **toevoegen** > **gebruikersnaam en wachtwoord (gescheiden)**.

Voer `ACR_ID` voor de **Gebruikersnaamvariabele**, en `ACR_PASSWORD` voor de **Wachtwoordvariabele**.

![Jenkins bindingen](media/aks-jenkins/bindings.png)

Voeg een **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst. Dit script maakt een nieuwe container installatiekopie en het verstuurd naar uw ACR-register.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Toevoegen van een andere **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst. Dit script werkt de Kubernetes-implementatie.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Wanneer het voltooid, klikt u op **opslaan**.

## <a name="test-the-jenkins-build"></a>De build Jenkins testen

Test de build Jenkins voordat u verdergaat. Hiermee valideert u of de taak build correct is geconfigureerd, het juiste Kubernetes verificatie-bestand aanwezig is en dat de juiste ACR-referenties zijn opgegeven.

Klik op **nu bouwen** in het menu links van het project.

![Jenkins testen build](media/aks-jenkins/test-build.png)

Tijdens dit proces is de GitHub-opslagplaats naar de server van de build Jenkins gekloond. Een nieuwe container-installatiekopie is gemaakt en naar het register ACR gepusht. Ten slotte wordt de toepassing Azure stem is uitgevoerd op het cluster AKS bijgewerkt voor het gebruik van de nieuwe installatiekopie. Omdat er geen wijzigingen hebt aangebracht aan de toepassingscode, wordt de toepassing niet gewijzigd.

Zodra het proces voltooid is, klikt u op in **bouwen #1** onder geschiedenis bouwen en selecteer **Console-uitvoer** voor een overzicht van alle uitvoer van de buildproces. De laatste regel zou moeten aangeven waarom een geslaagde build.

## <a name="create-github-webhook"></a>Een GitHub-webhook maken

Gebruik vervolgens de opslagplaats van toepassing op de server van de build Jenkins zodat op alle commit, een nieuwe build wordt geactiveerd.

1. Blader naar de Gevorkte GitHub-opslagplaats.
2. Selecteer **Instellingen**, selecteer daarna **Integraties en services** aan de linkerkant.
3. Kies **Service toevoegen**, voer `Jenkins (GitHub plugin)` in het filtervak en selecteer de invoegtoepassing.
4. Voer voor de Jenkins hook URL, `http://<publicIp:8080>/github-webhook/` waar `publicIp` het IP-adres van de server Jenkins. Zorg ervoor dat u de schuine /.
5. Selecteer toevoegen-service.

![GitHub-webhook](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>CI/CD-proces complete testen

Open op uw ontwikkelcomputer van de gekloonde toepassing met een code-editor.

Onder de **/azure-vote/azure-vote** directory, zoeken naar een bestand met de naam **config_file.cfg**. Werk de waarden stem in dit bestand op een andere waarde dan katten en honden.

Het volgende voorbeeld toont en bijgewerkt **config_file.cfg** bestand.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Als u klaar Sla het bestand, de wijzigingen ook aanbrengen en deze naar uw fork van de GitHub-opslagplaats pushen... Zodra het doorvoeren is voltooid, activeert de GitHub webhook een nieuwe build Jenkins, die de installatiekopie van het container en de implementatie AKS-updates. De procedure voor het maken van de beheerconsole Jenkins bewaken.

Zodra de build is voltooid, blader opnieuw naar het toepassingseindpunt van de op de wijzigingen bekijken.

![Azure stem bijgewerkt](media/aks-jenkins/azure-vote-updated-safari.png)

Op dit moment is een eenvoudige doorlopende implementatie voltooid. De stappen en configuraties die worden weergegeven in dit voorbeeld kunnen worden gebruikt voor het bouwen van een doorlopende build-automatisering krachtigere en gereed voor productie.

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az_acr_list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az_aks_get_credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli