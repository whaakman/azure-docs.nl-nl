---
title: Doorlopende Jenkins-implementatie met Kubernetes in Azure Kubernetes Service
description: Hoe u een continue implementatie automatiseren met Jenkins te implementeren en upgraden van een container geplaatste app in Kubernetes in Azure Kubernetes Service
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 246943b7e3df955394a6a79f9b3130633fe4ec50
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186610"
---
# <a name="continuous-deployment-with-jenkins-and-azure-kubernetes-service"></a>Doorlopende implementatie met Jenkins en Azure Kubernetes Service

Dit document laat zien hoe u een eenvoudige continue implementatiewerkstroom tussen Jenkins en een cluster Azure Kubernetes Service (AKS) instelt.

De voorbeeldwerkstroom bevat de volgende stappen uit:

> [!div class="checklist"]
> * De Azure vote-toepassing met uw Kubernetes-cluster implementeren.
> * Werk de code van de Azure vote-toepassing en pushen naar een GitHub-opslagplaats, waarmee het proces voor continue implementatie wordt gestart.
> * Jenkins kloont de opslagplaats en een nieuwe containerinstallatiekopie met de bijgewerkte code is gebaseerd.
> * Deze installatiekopie wordt gepusht naar Azure Container Registry (ACR).
> * De toepassing die wordt uitgevoerd in de AKS-cluster wordt bijgewerkt met de nieuwe containerinstallatiekopie.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om de stappen in dit artikel te voltooien.

- Basiskennis van Kubernetes, Git, CI/CD en Azure Container Registry (ACR).
- Een [cluster Azure Kubernetes Service (AKS)] [ aks-quickstart] en [AKS-referenties die zijn geconfigureerd] [ aks-credentials] op uw ontwikkelsysteem.
- Een [Azure Container Registry (ACR) register][acr-quickstart], de naam ACR-aanmeldingsserver, en [ACR referenties] [ acr-authentication] met push- en pull-toegang.
- Azure CLI is geïnstalleerd op uw ontwikkelsysteem.
- Docker is geïnstalleerd op uw ontwikkelsysteem.
- GitHub-account, [persoonlijk GitHub-toegangstoken][git-access-token], en Git-client is geïnstalleerd op uw ontwikkelsysteem.

## <a name="prepare-application"></a>Toepassing voorbereiden

De Azure vote-toepassing die wordt gebruikt in dit document bevat een webinterface die wordt gehost op een of meer schillen en een tweede schil hosting van Redis voor tijdelijke gegevensopslag.

Voordat u de via Jenkins bouwt / AKS-integratie, voorbereiden en implementeren van de Azure vote-toepassing naar uw AKS-cluster. Beschouw dit als versie 1 van de toepassing.

De volgende GitHub-opslagplaats splitsen.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Zodra de fork is gemaakt, deze klonen uw ontwikkelsysteem. Zorg ervoor dat u van de URL van uw fork gebruikmaakt bij het klonen van deze opslagplaats.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Wijzig de mappen zodat u vanuit de gekloonde map werkt.

```bash
cd azure-voting-app-redis
```

Voer de `docker-compose.yaml` bestand om te maken de `azure-vote-front` containerinstallatiekopie en start de toepassing.

```bash
docker-compose up -d
```

Wanneer dit is voltooid, gebruikt u de [docker-installatiekopieën] [ docker-images] opdracht om de gemaakte installatiekopie te bekijken.

U ziet dat er drie installatiekopieën zijn gedownload of gemaakt. De `azure-vote-front`-installatiekopie bevat de toepassing en gebruikt de `nginx-flask`-installatiekopie als basis. De `redis`-installatiekopie wordt gebruikt om een Redis-exemplaar te starten.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Ophalen van de ACR-aanmeldingsserver met de [az acr list] [ az-acr-list] opdracht. Zorg ervoor dat u de naam van de resourcegroep met de resourcegroep die als host fungeert voor uw ACR-register bijwerken.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Gebruik de [docker tag] [ docker-tag] opdracht voor het taggen van de installatiekopie met de naam van de aanmeldingsserver en een uniek versienummer van `v1`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Werk de waarde van de ACR-aanmelding met uw naam ACR-aanmeldingsserver en push de `azure-vote-front` installatiekopie naar het register.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Toepassing implementeren in Kubernetes

Een Kubernetes-manifestbestand kan worden gevonden in de hoofdmap van de Azure vote-opslagplaats en kan worden gebruikt om de toepassing implementeren in uw Kubernetes-cluster.

Werk eerst de **azure-vote-all-in-one-redis.yaml** manifestbestand met de locatie van uw ACR-register. Open het bestand met een teksteditor en vervang `microsoft` met de naam van de ACR-aanmeldingsserver. U vindt deze waarde op regel **47** van het manifestbestand.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Gebruik vervolgens de [kubectl toepassen] [ kubectl-apply] opdracht uit te voeren van de toepassing. Deze opdracht parseert het manifestbestand en maakt de gedefinieerde Kubernetes-objecten.

```bash
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Een [Kubernetes-service] [ kubernetes-service] wordt gemaakt om de toepassing op het internet zichtbaar te maken. Dit proces kan enkele minuten duren.

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

## <a name="deploy-jenkins-to-vm"></a>Implementeren van Jenkins met virtuele machine

Een script is vooraf gemaakte aan een virtuele machine implementeren, configureren van toegang tot het netwerk en een eenvoudige Jenkins-installatie te voltooien. Het script opgehaald bovendien uw Kubernetes-configuratiebestand uit uw ontwikkelsysteem aan het Jenkins-systeem. Dit bestand wordt gebruikt voor verificatie tussen Jenkins en het AKS-cluster.

Voer de volgende opdrachten downloaden en uitvoeren van het script. De onderstaande URL kan ook worden gebruikt om de inhoud van het script te controleren.

> [!WARNING]
> Met dit voorbeeldscript wordt voor deze demo voor het snel inrichten van een Jenkins-omgeving die wordt uitgevoerd op een Azure-VM. Het maakt gebruik van de aangepaste scriptextensie van Azure voor het configureren van een virtuele machine en vervolgens de vereiste referenties weer te geven. Uw *~/.kube/config* wordt gekopieerd naar de Jenkins-VM.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Wanneer het script is voltooid, voert deze een adres voor de Jenkins-server, als ook een sleutel voor het ontgrendelen van Jenkins. Blader naar de URL, voer de sleutel en volgt de instructies om de Jenkins-configuratie te voltooien.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Jenkins-omgevingsvariabelen

Een Jenkins-omgevingsvariabele wordt gebruikt voor het opslaan van de aanmeldingsnaam voor Azure Container Registry (ACR). Deze variabele wordt verwezen tijdens de Jenkins-taak met continue implementatie.

Op de Jenkins-beheerportal, klikt u op **Manage Jenkins** > **Configure System**.

Onder **algemene eigenschappen**, selecteer **omgevingsvariabelen**, en het toevoegen van een variabele met de naam van de `ACR_LOGINSERVER` en een waarde van uw ACR-aanmeldingsserver.

![Jenkins-omgevingsvariabelen](media/aks-jenkins/env-variables.png)

Als u klaar bent, klikt u op **opslaan** op de pagina van de Jenkins-configuratie.

## <a name="jenkins-credentials"></a>Jenkins-referenties

Nu uw ACR-referenties opslaan in een Jenkins-referentieobject. Deze referenties worden verwezen bij de Jenkins-build-taak.

Klik op terug op de Jenkins-beheerportal, **referenties** > **Jenkins** > **algemene aanmeldgegevens (onbeperkte)**  >   **Referenties toevoegen**.

Zorg ervoor dat het type referentie **gebruikersnaam met wachtwoord** en voer de volgende items:

- **Gebruikersnaam** -ID van de service-principal gebruiken voor verificatie met uw ACR-register.
- **Wachtwoord** -clientgeheim van de service-principal gebruiken voor verificatie met uw ACR-register.
- **ID** -referentie-id zoals `acr-credentials`.

Als u klaar bent, het formulier referenties moet als volgt uitzien in deze afbeelding:

![ACR-referenties](media/aks-jenkins/acr-credentials.png)

Klik op **OK** en Ga terug naar de Jenkins-beheerportal.

## <a name="create-jenkins-project"></a>Jenkins-project maken

Klik in de Jenkins-beheerportal, op **Nieuw Item**.

Geef het project een naam, bijvoorbeeld `azure-vote`, selecteer **Freestyle Project**, en klikt u op **OK**.

![Jenkins-project](media/aks-jenkins/jenkins-project.png)

Onder **algemene**, selecteer **GitHub-project** en voer de URL naar uw fork van de Azure vote-GitHub-project.

![GitHub-project](media/aks-jenkins/github-project.png)

Onder **Source Code Management**, selecteer **Git**, voer de URL naar uw fork van de Azure Vote-GitHub-opslagplaats.

Klik op voor de referenties en **toevoegen** > **Jenkins**. Onder **soort**, selecteer **geheime tekst** en voer uw [persoonlijk GitHub-toegangstoken] [ git-access-token] als de geheime sleutel.

Selecteer **toevoegen** wanneer u klaar bent.

![GitHub-referenties](media/aks-jenkins/github-creds.png)

Onder **Triggers bouwen**, selecteer **GitHub-hookactivatie voor GITScm-polling**.

![Jenkins-build triggers](media/aks-jenkins/build-triggers.png)

Onder **omgeving bouwen**, selecteer **gebruik geheime teksten of bestanden**.

![Jenkins build-omgeving](media/aks-jenkins/build-environment.png)

Onder **bindingen**, selecteer **toevoegen** > **gebruikersnaam en wachtwoord (gescheiden)**.

Voer `ACR_ID` voor de **Gebruikersnaamvariabele**, en `ACR_PASSWORD` voor de **Wachtwoordvariabele**.

![Jenkins-bindingen](media/aks-jenkins/bindings.png)

Voeg een **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst. Met dit script maakt een nieuwe containerinstallatiekopie en wordt naar uw ACR-register gepusht.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Voeg nog een **bouwen stap** van het type **shell uitvoeren** en gebruik de volgende tekst. Met dit script werkt de Kubernetes-implementatie.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Wanneer voltooid, klikt u op **opslaan**.

## <a name="test-the-jenkins-build"></a>Testen van de Jenkins-build

Voordat u doorgaat, test u de Jenkins-build. Hiermee valideert u dat de build-taak correct is geconfigureerd, het juiste verificatiebestand van Kubernetes is voldaan en dat de juiste ACR-referenties hebt opgegeven.

Klik op **Build Now** in het menu links van het project.

![Jenkins testen build](media/aks-jenkins/test-build.png)

Tijdens dit proces wordt de GitHub-opslagplaats gekloond naar de Jenkins-build-server. Een nieuwe containerinstallatiekopie is gemaakt en naar het ACR-register gepusht. Ten slotte wordt de Azure vote-toepassing die wordt uitgevoerd op het AKS-cluster bijgewerkt voor het gebruik van de nieuwe installatiekopie. Omdat er geen wijzigingen hebt aangebracht in de toepassingscode, wordt de toepassing niet gewijzigd.

Als het proces voltooid is, klikt u op **bouwen #1** onder build-geschiedenis en selecteer **Console-uitvoer** alle uitvoer van het bouwproces. De laatste regel moet een build aangeven.

## <a name="create-github-webhook"></a>Een GitHub-webhook maken

Vervolgens koppelt u de toepassingsopslagplaats van de naar de Jenkins-build-server zodat op een willekeurige doorvoering, een nieuwe build is geactiveerd.

1. Blader naar de Gevorkte GitHub-opslagplaats.
2. Selecteer **instellingen**en selecteer vervolgens **Webhooks** aan de linkerkant.
3. Kies aan **webhook toevoegen**. Voor de *nettolading URL*, voer `http://<publicIp:8080>/github-webhook/` waar `publicIp` is het IP-adres van de Jenkins-server. Zorg ervoor dat u de afsluitende /. Laat de overige standaardwaarden voor type inhoud en activeren op *push* gebeurtenissen.
4. Selecteer **webhook toevoegen**.

    ![GitHub-webhook](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>CI/CD-proces end-to-end testen

Op uw ontwikkelcomputer, opent u de gekloonde toepassing met een code-editor.

Onder de **/azure-vote/azure-vote** directory, zoeken naar een bestand met de naam **config_file.cfg**. Werk de waarden stemming in dit bestand op een andere waarde dan katten en honden.

Het volgende voorbeeld laat zien en bijgewerkt **config_file.cfg** bestand.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Als u klaar bent, sla het bestand, de wijzigingen aan en deze naar uw fork van de GitHub-opslagplaats te pushen... Als het doorvoeren is voltooid, wordt een nieuwe Jenkins-build, die updates van de container-installatiekopie en de AKS-implementatie in de GitHub-webhook geactiveerd. Het buildproces op de Jenkins-beheerconsole controleren.

Als de build is voltooid, blader nogmaals naar het toepassingseindpunt van de op Bekijk de wijzigingen.

![Azure vote bijgewerkt](media/aks-jenkins/azure-vote-updated-safari.png)

Op dit moment is een eenvoudige continue implementatie-proces voltooid. De stappen en configuraties die worden weergegeven in dit voorbeeld kunnen worden gebruikt voor het bouwen van een meer robuuste en gereed is voor productie doorlopend bouwen automation.

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az_acr_list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az_aks_get_credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli